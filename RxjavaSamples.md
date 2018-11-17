# RxJava 2 和 Retrofit 结合使用的几个最常见使用方式举例 #

# 基本使用 #

RxJava 和 Retrofit 结合使用最基本的格式：用 subscribeOn() 和 observeOn() 来控制线程，并通过 subscribe() 来触发网络请求的开始。代码大致形式：

	disposable = Network.getZhuangbiApi()
                .search(key)
                .subscribeOn(Schedulers.io())
                .observeOn(AndroidSchedulers.mainThread())
                .subscribe(new Consumer<List<ZhuangbiImage>>() {
                    @Override
                    public void accept(@NonNull List<ZhuangbiImage> images) throws Exception {
                        swipeRefreshLayout.setRefreshing(false);
                        adapter.setImages(images);
                    }
                }, new Consumer<Throwable>() {
                    @Override
                    public void accept(@NonNull Throwable throwable) throws Exception {
                        swipeRefreshLayout.setRefreshing(false);
                        Toast.makeText(getActivity(), R.string.loading_failed, Toast.LENGTH_SHORT).show();
                    }
                });

# map #

有些服务端的接口设计，会在返回的数据外层包裹一些额外信息，这些信息对于调试很有用，但本地显示是用不到的。使用 map() 可以把外层的格式剥掉，只留下本地会用到的核心格式。

	disposable = Network.getGankApi()
                .getBeauties(10, page)
                .map(GankBeautyResultToItemsMapper.getInstance())
                .subscribeOn(Schedulers.io())
                .observeOn(AndroidSchedulers.mainThread())
                .subscribe(new Consumer<List<Item>>() {
                    @Override
                    public void accept(@NonNull List<Item> items) throws Exception {
                        swipeRefreshLayout.setRefreshing(false);
                        pageTv.setText(getString(R.string.page_with_number, MapFragment.this.page));
                        adapter.setItems(items);
                    }
                }, new Consumer<Throwable>() {
                    @Override
                    public void accept(@NonNull Throwable throwable) throws Exception {
                        swipeRefreshLayout.setRefreshing(false);
                        Toast.makeText(getActivity(), R.string.loading_failed, Toast.LENGTH_SHORT).show();
                    }
                });

    public class GankBeautyResultToItemsMapper implements Function<GankBeautyResult, List<Item>> {
	    private static GankBeautyResultToItemsMapper INSTANCE = new GankBeautyResultToItemsMapper();
	
	    private GankBeautyResultToItemsMapper() {
	    }
	
	    public static GankBeautyResultToItemsMapper getInstance() {
	        return INSTANCE;
	    }
	
	    @Override
	    public List<Item> apply(GankBeautyResult gankBeautyResult) {
	        List<GankBeauty> gankBeauties = gankBeautyResult.beauties;
	        List<Item> items = new ArrayList<>(gankBeauties.size());
	        SimpleDateFormat inputFormat = new SimpleDateFormat("yyyy-MM-dd'T'HH:mm:ss.SS'Z'");
	        SimpleDateFormat outputFormat = new SimpleDateFormat("yy/MM/dd HH:mm:ss");
	        for (GankBeauty gankBeauty : gankBeauties) {
	            Item item = new Item();
	            try {
	                Date date = inputFormat.parse(gankBeauty.createdAt);
	                item.description = outputFormat.format(date);
	            } catch (ParseException e) {
	                e.printStackTrace();
	                item.description = "unknown date";
	            }
	            item.imageUrl = gankBeauty.url;
	            items.add(item);
	        }
	        return items;
	    }
	}

# zip #

有的时候，app 中会需要同时访问不同接口，然后将结果糅合后转为统一的格式后输出（例如将第三方广告 API 的广告夹杂进自家平台返回的数据 List 中）。这种并行的异步处理比较麻烦，不过用了 zip() 之后就会简单得多。


    disposable = Observable.zip(Network.getGankApi().getBeauties(200, 1).map(GankBeautyResultToItemsMapper.getInstance()),
                Network.getZhuangbiApi().search("装逼"),
                new BiFunction<List<Item>, List<ZhuangbiImage>, List<Item>>() {
                    @Override
                    public List<Item> apply(List<Item> gankItems, List<ZhuangbiImage> zhuangbiImages) {
                        List<Item> items = new ArrayList<Item>();
                        for (int i = 0; i < gankItems.size() / 2 && i < zhuangbiImages.size(); i++) {
                            items.add(gankItems.get(i * 2));
                            items.add(gankItems.get(i * 2 + 1));
                            Item zhuangbiItem = new Item();
                            ZhuangbiImage zhuangbiImage = zhuangbiImages.get(i);
                            zhuangbiItem.description = zhuangbiImage.description;
                            zhuangbiItem.imageUrl = zhuangbiImage.image_url;
                            items.add(zhuangbiItem);
                        }
                        return items;
                    }
                })
                .subscribeOn(Schedulers.io())
                .observeOn(AndroidSchedulers.mainThread())
                .subscribe(new Consumer<List<Item>>() {
                    @Override
                    public void accept(@NonNull List<Item> items) throws Exception {
                        swipeRefreshLayout.setRefreshing(false);
                        adapter.setItems(items);
                    }
                }, new Consumer<Throwable>() {
                    @Override
                    public void accept(@NonNull Throwable throwable) throws Exception {
                        swipeRefreshLayout.setRefreshing(false);
                        Toast.makeText(getActivity(), R.string.loading_failed, Toast.LENGTH_SHORT).show();
                    }
                });

# 一次性token(flatmap) #

出于安全性、性能等方面的考虑，多数服务器会有一些接口需要传入 token 才能正确返回结果，而 token 是需要从另一个接口获取的，这就需要使用两步连续的请求才能获取数据（①token -> ②目标数据）。使用 flatMap() 可以用较为清晰的代码实现这种连续请求，避免 Callback 嵌套的结构。

    disposable = fakeApi.getFakeToken("fake_auth_code")
                .flatMap(new Function<FakeToken, Observable<FakeThing>>() {
                    @Override
                    public Observable<FakeThing> apply(FakeToken fakeToken) {
                        return fakeApi.getFakeData(fakeToken);
                    }
                })
                .subscribeOn(Schedulers.io())
                .observeOn(AndroidSchedulers.mainThread())
                .subscribe(new Consumer<FakeThing>() {
                    @Override
                    public void accept(FakeThing fakeData) {
                        swipeRefreshLayout.setRefreshing(false);
                        tokenTv.setText(getString(R.string.got_data, fakeData.id, fakeData.name));
                    }
                }, new Consumer<Throwable>() {
                    @Override
                    public void accept(Throwable throwable) {
                        swipeRefreshLayout.setRefreshing(false);
                        Toast.makeText(getActivity(), R.string.loading_failed, Toast.LENGTH_SHORT).show();
                    }
                });

# 非一次性token #

有的 token 并非一次性的，而是可以多次使用，直到它超时或被销毁（多数 token 都是这样的）。这样的 token 处理起来比较麻烦：需要把它保存起来，并且在发现它失效的时候要能够自动重新获取新的 token 并继续访问之前由于 token 失效而失败的请求。如果项目中有多处的接口请求都需要这样的自动修复机制，使用传统的 Callback 形式需要写出非常复杂的代码。而使用 RxJava ，可以用 retryWhen() 来轻松地处理这样的问题。

    disposable = Observable.just(1)
                .flatMap(new Function<Object, Observable<FakeThing>>() {
                    @Override
                    public Observable<FakeThing> apply(Object o) {
                        return cachedFakeToken.token == null
                                ? Observable.<FakeThing>error(new NullPointerException("Token is null!"))
                                : fakeApi.getFakeData(cachedFakeToken);
                    }
                })
                .retryWhen(new Function<Observable<? extends Throwable>, Observable<?>>() {
                    @Override
                    public Observable<?> apply(Observable<? extends Throwable> observable) {
                        return observable.flatMap(new Function<Throwable, Observable<?>>() {
                            @Override
                            public Observable<?> apply(Throwable throwable) {
                                if (throwable instanceof IllegalArgumentException || throwable instanceof NullPointerException) {
                                    return fakeApi.getFakeToken("fake_auth_code")
                                            .doOnNext(new Consumer<FakeToken>() {
                                                @Override
                                                public void accept(FakeToken fakeToken) {
                                                    tokenUpdated = true;
                                                    cachedFakeToken.token = fakeToken.token;
                                                    cachedFakeToken.expired = fakeToken.expired;
                                                }
                                            });
                                }
                                return Observable.error(throwable);
                            }
                        });
                    }
                })
                .subscribeOn(Schedulers.io())
                .observeOn(AndroidSchedulers.mainThread())
                .subscribe(new Consumer<FakeThing>() {
                    @Override
                    public void accept(FakeThing fakeData) {
                        swipeRefreshLayout.setRefreshing(false);
                        String token = cachedFakeToken.token;
                        if (tokenUpdated) {
                            token += "(" + getString(R.string.updated) + ")";
                        }
                        tokenTv.setText(getString(R.string.got_token_and_data, token, fakeData.id, fakeData.name));
                    }
                }, new Consumer<Throwable>() {
                    @Override
                    public void accept(Throwable throwable) {
                        swipeRefreshLayout.setRefreshing(false);
                        Toast.makeText(getActivity(), R.string.loading_failed, Toast.LENGTH_SHORT).show();
                    }
                });

# 缓存 #

RxJava 中有一个较少被人用到的类叫做 Subject，它是一种『既是 Observable，又是 Observer』的东西，因此可以被用作中间件来做数据传递。例如，可以用它的子类 BehaviorSubject 来制作缓存.





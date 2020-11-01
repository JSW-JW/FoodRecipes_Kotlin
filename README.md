# penetrate structure in this app
<br>
* Sequence the app is executed.
<br>
1. (NetworkBoundResource, LiveDataCallAdapter, LiveDataCallAdapterFactory) -> create ApiResponse<T>
2. According to the functions that will implemented in repository, they will be executed when Retrofit call is made.
3. The 'sequence that response is retrieved and inserted into db and loaded' is (1)shouldFetch(), (2)createCall, (3)saveCallResult, (4)loadFromDb()
4. If ApiResponse.ApiSuccessResponse, it is saved in local db on background thread.(diskIO)
5. In cases of ApiResponse(3 cases) -> loadDb(), setValue and return MediatorLiveData<Resource<CacheObject>>) (e.g : LiveData<Resource<Recipe>> or LiveData<Resource<List<Recipe>>>)
6. Repository returns LiveData<Resource<Recipe>> or LiveData<Resource<List<Recipe>>>.(which is retrieved from NetworkBoundResource)
7. Several view models (it depends on the case of app) get the repositoryResource and return it.
8. In activity, LiveDatas are subscribed and update the view.

# the reason this app is using Generic type.

* it is using many generic type in classes like NetworkBoundResource, ApiResponse, Resource in Util Package.
* this facilitates the usage of various ApiCall and as a result it reduces the total amount of code and makes it more readable, maintainable.

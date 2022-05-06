# Multiplatform Paging

This library packages [Jetpack Paging] for Kotlin/Multiplatform.

Our multiplatform APIs use `app.cash.paging`.
This library delegates to the stock Jetpack Paging artifact on the Android platform.

## Building This Project

This project uses Git submodules to build our branch of Jetpack paging alongside our common API declarations.

You'll need to run these commands before you can build:

```
$ git submodule init
```

## Directory structure

- paging/src/commonMain
  - Defines the common multiplatform API.
    This is the paging3 subset of `androidx.paging`.
  - Exclusively `expect` types with package name `app.cash.paging`.

- paging/src/jvmMain
  - Only `typealias` declarations from `app.cash.paging` to `androidx.paging`.
  - This has a library dependency on androidx.paging.
    We depend on Google's official releases on Android.

- paging/src/nonJvmMain
  - Only `typealias` declarations from `app.cash.paging` to `androidx.paging`.
  - Includes sources from our branch of androidx.paging in the `upstreams/androidx-main-mpp` directory.

- upstreams/androidx-main-mpp
  - the androidx repo, branched to work on non-JVM platforms
  - our branch does this:
    - prunes everything that isn't paging-related
    - changes paging internals to support non-JVM platforms

Note that commonMain includes only the Paging 3 APIs from `androidx.paging`.
We don't plan to offer multiplatform APIs for Paging 2.

## API Discrepancies

Our common API is the same as `androidx.paging`, copied to `app.cash.paging`.
All types in `app.cash.paging` typealias to `androidx.paging` on all platforms.

Unfortunately, there are a few exceptions, due to limitations in the Kotlin compiler.

| `androidx.paging` API                       | `app.cash.paging` API                     | Issue reference |
|---------------------------------------------|-------------------------------------------|-----------------|
| LoadState.NotLoading                        | LoadStateNotLoading                       |                 |
| LoadState.Loading                           | LoadStateLoading                          |                 |
| LoadState.Error                             | LoadStateError                            |                 |
| PagingSource.LoadParams<Key>                | PagingSourceLoadParams<Key>               |                 |
| PagingSource.LoadParams.Refresh<Key>        | PagingSourceLoadParamsRefresh<Key>        |                 |
| PagingSource.LoadParams.Append<Key>         | PagingSourceLoadParamsAppend<Key>         |                 |
| PagingSource.LoadParams.Prepend<Key>        | PagingSourceLoadParamsPrepend<Key>        |                 |
| PagingSource.LoadResult.Error<Key, Value>   | PagingSourceLoadResultError<Key, Value>   |                 |
| PagingSource.LoadResult.Invalid<Key, Value> | PagingSourceLoadResultInvalid<Key, Value> |                 |
| PagingSource.LoadResult.Page<Key, Value>    | PagingSourceLoadResultPage<Key, Value>    |                 |
| RemoteMediator.MediatorResult               | MediatorResult                            |                 |
| RemoteMediator.MediatorResult.Error         | MediatorResultError                       |                 |
| RemoteMediator.MediatorResult.Success       | MediatorResultSuccess                     |                 |
| RemoteMediator.InitializeAction             | InitializeAction                          |                 |

## Versioning

Multiplatform Paging follows the [same version numbers](https://mvnrepository.com/artifact/androidx.paging/paging-common) as Jetpack's Paging.
We will (or won't?) follow alpha/beta/rc releases of androidx/paging.
In the case of patch releases affecting just Multiplatform Paging, we will append `-patchX` (e.g., `app.cash.paging:paging-common:3.1.1-patch01`).
Catching up to the latest release of Jetpack's Paging may take some time due to potential issues making the new KMP compatible.
A tracking issue will be created and pinned each time we are lagging behind a version.

It's similar to the [versioning of KSP](https://mvnrepository.com/artifact/com.google.devtools.ksp/symbol-processing) where it's the androidx-paging version number followed by our number.

[Jetpack Paging]: https://developer.android.com/topic/libraries/architecture/paging/v3-overview

## License

    Copyright 2022 Block, Inc.

    Licensed under the Apache License, Version 2.0 (the "License");
    you may not use this file except in compliance with the License.
    You may obtain a copy of the License at

       http://www.apache.org/licenses/LICENSE-2.0

    Unless required by applicable law or agreed to in writing, software
    distributed under the License is distributed on an "AS IS" BASIS,
    WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
    See the License for the specific language governing permissions and
    limitations under the License.
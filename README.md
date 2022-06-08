# raylib-emscripten
Template for making Raylib projects using emscripten, and using them in github pages.

```
mkdir em-build
cd em-build
emcmake cmake .. -DPLATFORM=Web -DCMAKE_BUILD_TYPE=Release -DCMAKE_EXE_LINKER_FLAGS="-s USE_GLFW=3"
emmake make
emrun ../index.html
```

In order to make a GitHub Pages, copy the index.html, em-build/main.js, and em-build/main.wasm into the gh-pages repository.

FROM emscripten/emsdk:3.1.17 as build

RUN apt-get update
RUN apt-get install -y libasound2-dev libjack-jackd2-dev \
    ladspa-sdk \
    libcurl4-openssl-dev  \
    libfreetype6-dev \
    libx11-dev libxcomposite-dev libxcursor-dev libxcursor-dev libxext-dev libxinerama-dev libxrandr-dev libxrender-dev \
    libwebkit2gtk-4.0-dev \
    libglu1-mesa-dev mesa-common-dev

WORKDIR /app

COPY ./examples ./examples
COPY ./extras ./extras
COPY ./modules ./modules
COPY ./docs ./docs

WORKDIR /app/examples/DemoRunner/Builds/Emscripten

RUN emmake make

FROM nginx:1.19.7-alpine as dev

COPY ./docker/proxy/nginx.conf /etc/nginx/nginx.conf
COPY --from=build /app/examples/DemoRunner/Builds/Emscripten/build/ /frontend/

CMD ["nginx", "-g", "daemon off;"] 
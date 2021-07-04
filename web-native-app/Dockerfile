FROM ghcr.io/graalvm/graalvm-ce:latest as build-aot

RUN gu install native-image
WORKDIR /workdir

COPY target/*-fat.jar app.jar

RUN native-image -cp app.jar \
    --no-server \
    --no-fallback \
    --enable-all-security-services \
    --allow-incomplete-classpath \
    --initialize-at-run-time=io.netty.handler.codec.compression.BrotliDecoder \
    --initialize-at-run-time=io.netty.util.internal.logging.Log4JLogger \
    --initialize-at-run-time=io.netty.handler.codec.compression.BrotliDecoder$1 \
    --initialize-at-run-time=io.netty.handler.ssl.JettyNpnSslEngine \
    --initialize-at-run-time=io.netty.handler.ssl.OpenSslSessionContext \
    --initialize-at-run-time=io.netty.handler.ssl.ReferenceCountedOpenSslContext \
    --initialize-at-run-time=io.netty.handler.ssl.JdkNpnApplicationProtocolNegotiator \
    --initialize-at-run-time=io.netty.util.internal.logging.Slf4JLoggerFactory \
    --initialize-at-run-time=io.netty.handler.ssl.JettyAlpnSslEngine$ClientEngine \
    --initialize-at-run-time=io.netty.handler.ssl.ConscryptAlpnSslEngine \
    --initialize-at-run-time=io.netty.handler.ssl.ReferenceCountedOpenSslEngine \
    --initialize-at-run-time=io.netty.handler.ssl.JettyAlpnSslEngine$ServerEngine \
    -H:Name="app" \
    com.udemy.aws.eks.masterclass.MainVerticle

RUN chmod +x app

# RUN ls -la

FROM debian:10.9-slim
COPY --from=build-aot /workdir/app app

EXPOSE 8888
CMD ["./app"]

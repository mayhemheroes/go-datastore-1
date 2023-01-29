FROM golang:1.17.13 as gofuzzbuilder
ARG project='go-datastore'
ARG fuzzTargetPath='fuzz'

ADD . ${project}
WORKDIR ${project}/${fuzzTargetPath}
RUN go get -u github.com/dvyukov/go-fuzz/go-fuzz@latest github.com/dvyukov/go-fuzz/go-fuzz-build@latest
RUN GO_ENABLED=0 go-fuzz-build -libfuzzer -o /query-fuzz.a .

FROM silkeh/clang:latest as sanitizer
COPY --from=gofuzzbuilder /query-fuzz.a query-fuzz.a
RUN clang -fsanitize=fuzzer /query-fuzz.a -o /query-fuzz.libfuzzer

FROM ubuntu:20.04
COPY --from=sanitizer /query-fuzz.libfuzzer /

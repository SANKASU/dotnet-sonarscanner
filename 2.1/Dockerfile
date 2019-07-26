FROM openjdk:8u212-jre-alpine3.9 AS ci-build

RUN apk add --no-cache \
        ca-certificates \
        \
        krb5-libs \
        libgcc \
        libintl \
        libssl1.1 \
        libstdc++ \
        lttng-ust \
        tzdata \
        userspace-rcu \
        zlib \
        icu-libs

ENV ASPNETCORE_URLS=http://+:80 \
    DOTNET_RUNNING_IN_CONTAINER=true \
    DOTNET_CLI_TELEMETRY_OPTOUT=true \
    LC_ALL=en_US.UTF-8 \
    LANG=en_US.UTF-8

ENV DOTNET_SDK_VERSION 2.1.801

RUN wget -O dotnet.tar.gz https://dotnetcli.blob.core.windows.net/dotnet/Sdk/$DOTNET_SDK_VERSION/dotnet-sdk-$DOTNET_SDK_VERSION-linux-musl-x64.tar.gz \
    && dotnet_sha512='a3f9044d04a18beb6dcbae5a91bbeadb38b17870154b91caf201e8656c44325d43c24aa0bb5f7a934a818c3a403ed2a2af428f456fb63a21723dc50988f788e7' \
    && echo "$dotnet_sha512  dotnet.tar.gz" | sha512sum -c - \
    && mkdir -p /usr/share/dotnet \
    && tar -C /usr/share/dotnet -xzf dotnet.tar.gz \
    && ln -s /usr/share/dotnet/dotnet /usr/bin/dotnet \
    && rm dotnet.tar.gz

# Enable correct mode for dotnet watch (only mode supported in a container)
ENV DOTNET_USE_POLLING_FILE_WATCHER=true \
    NUGET_XMLDOC_MODE=skip

RUN dotnet tool install -g dotnet-sonarscanner
ENV PATH="${PATH}:/root/.dotnet/tools"

RUN dotnet help

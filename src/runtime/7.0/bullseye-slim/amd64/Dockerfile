ARG REPO=mcr.microsoft.com/dotnet/runtime-deps

# Installer image
FROM amd64/buildpack-deps:bullseye-curl AS installer

# Retrieve .NET Runtime
RUN dotnet_version=7.0.14 \
    && curl -fSL --output dotnet.tar.gz https://dotnetcli.azureedge.net/dotnet/Runtime/$dotnet_version/dotnet-runtime-$dotnet_version-linux-x64.tar.gz \
    && dotnet_sha512='02fd66ef2059d124d9c4f3fbfd0d5b0375b83610cdf51a2972567e4bdaf1d55e532478533509ec2408c371e7fdd6efea8e9b9aec9eb5cd703e8e5d2814ef319b' \
    && echo "$dotnet_sha512  dotnet.tar.gz" | sha512sum -c - \
    && mkdir -p /dotnet \
    && tar -oxzf dotnet.tar.gz -C /dotnet \
    && rm dotnet.tar.gz


# .NET runtime image
FROM $REPO:7.0.14-bullseye-slim-amd64

# .NET Runtime version
ENV DOTNET_VERSION=7.0.14

COPY --from=installer ["/dotnet", "/usr/share/dotnet"]

RUN ln -s /usr/share/dotnet/dotnet /usr/bin/dotnet

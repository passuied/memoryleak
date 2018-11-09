FROM microsoft/dotnet:2.1.5-aspnetcore-runtime-stretch-slim AS base-env
WORKDIR /app
EXPOSE 8080


# intermediate steps
#build
FROM microsoft/dotnet:2.1-sdk AS build-env
WORKDIR /appcode
COPY . ./

#publish
FROM build-env AS publish-env

WORKDIR /appcode/src/MemoryLeak/MemoryLeak
RUN dotnet publish -c Release -o /app

# Build runtime image
FROM base-env AS final-env

WORKDIR /app
COPY --from=publish-env /app .

RUN groupadd -g 999 csodservices && \
    useradd -r -u 999 -g csodservices csodservices

USER csodservices

CMD ["dotnet","Poc.Rest.dll"]

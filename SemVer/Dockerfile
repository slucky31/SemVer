FROM --platform=$BUILDPLATFORM mcr.microsoft.com/dotnet/sdk:8.0-jammy AS build
ARG TARGETARCH
WORKDIR /source

# copy csproj and restore as distinct layers
COPY SemVer.sln .
COPY SemVer/SemVer.csproj ./SemVer/
RUN dotnet restore "./SemVer/SemVer.csproj" -a $TARGETARCH

# copy everything else and build app
COPY . .
RUN dotnet publish "./SemVer/SemVer.csproj" -a $TARGETARCH -o /app

# final stage/image
FROM mcr.microsoft.com/dotnet/aspnet:8.0-jammy
EXPOSE 8080
WORKDIR /app
COPY --from=build /app .
USER $APP_UID
ENTRYPOINT ["./SemVer"]

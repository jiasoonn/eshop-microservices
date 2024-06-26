FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
WORKDIR /app
EXPOSE 5000

ENV ASPNETCORE_URLS=http://+:5000

USER app
FROM --platform=$BUILDPLATFORM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG configuration=Release
WORKDIR /src
COPY ["src/services/catalog/catalog.api/catalog.api.csproj", "src/services/catalog/catalog.api/"]
RUN dotnet restore "src/services/catalog/catalog.api/catalog.api.csproj"
COPY . .
WORKDIR "/src/src/services/catalog/catalog.api"
RUN dotnet build "catalog.api.csproj" -c $configuration -o /app/build

FROM build AS publish
ARG configuration=Release
RUN dotnet publish "catalog.api.csproj" -c $configuration -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "catalog.api.dll"]

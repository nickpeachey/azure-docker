FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
USER $APP_UID
WORKDIR /app
EXPOSE 8080
EXPOSE 80
EXPOSE 8081

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["Azure-Docker.Api/Azure-Docker.Api.csproj", "Azure-Docker.Api/"]
RUN dotnet restore "Azure-Docker.Api/Azure-Docker.Api.csproj"
COPY . .
WORKDIR "/src/Azure-Docker.Api"
RUN dotnet build "Azure-Docker.Api.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "Azure-Docker.Api.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Azure-Docker.Api.dll"]

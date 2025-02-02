FROM mcr.microsoft.com/dotnet/aspnet:9.0 AS base
USER $APP_UID
WORKDIR /app
EXPOSE 80


FROM mcr.microsoft.com/dotnet/sdk:9.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["ZhilKomStroy/ZhilKomStroy.csproj", "ZhilKomStroy/"]
RUN dotnet restore "ZhilKomStroy/ZhilKomStroy.csproj"
COPY . .
WORKDIR "/src/ZhilKomStroy"
RUN dotnet build "ZhilKomStroy.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "ZhilKomStroy.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "ZhilKomStroy.dll"]

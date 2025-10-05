FROM nginx AS base
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:9.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["KargoDemo/KargoDemo.csproj", "KargoDemo/"]
RUN dotnet restore "./KargoDemo/KargoDemo.csproj"
COPY . .
WORKDIR "/src/KargoDemo"
RUN dotnet build "./KargoDemo.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "./KargoDemo.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM  base AS final
WORKDIR /usr/share/nginx/html
COPY --from=publish /app/publish/wwwroot .
COPY  KargoDemo/nginx.conf /etc/nginx/nginx.conf
FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
WORKDIR /src
COPY ["custom-middleware-ssr/custom-middleware-ssr.csproj", "custom-middleware-ssr/"]
RUN dotnet restore "custom-middleware-ssr/custom-middleware-ssr.csproj"
COPY . .
WORKDIR "/src/custom-middleware-ssr"
RUN dotnet build "custom-middleware-ssr.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "custom-middleware-ssr.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "custom-middleware-ssr.dll"]

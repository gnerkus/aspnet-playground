FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
WORKDIR /src
COPY ["custom-middleware/custom-middleware.csproj", "custom-middleware/"]
RUN dotnet restore "custom-middleware/custom-middleware.csproj"
COPY . .
WORKDIR "/src/custom-middleware"
RUN dotnet build "custom-middleware.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "custom-middleware.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "custom-middleware.dll"]

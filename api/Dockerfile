FROM mcr.microsoft.com/dotnet/core/aspnet:3.1 AS base
WORKDIR /app
EXPOSE 80

FROM mcr.microsoft.com/dotnet/core/sdk:3.1 AS build
WORKDIR /src
COPY ["api/Logging.csproj", "api/"]
RUN dotnet restore "api/Logging.csproj"
COPY . .
WORKDIR "/src/api"
RUN dotnet build "Logging.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "Logging.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Logging.dll"]

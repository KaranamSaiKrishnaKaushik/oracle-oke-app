# Build stage
FROM mcr.microsoft.com/dotnet/sdk:9.0 AS build
WORKDIR /src

# Copy csproj and restore (better caching)
COPY backend-oke-api-service/*.csproj ./backend-oke-api-service/
RUN dotnet restore "backend-oke-api-service/backend-oke-api-service.csproj"

# Copy the rest and build
COPY backend-oke-api-service/ ./backend-oke-api-service/
WORKDIR /src/backend-oke-api-service
RUN dotnet build "backend-oke-api-service.csproj" -c Release -o /app/build

# Publish stage
FROM build AS publish
RUN dotnet publish "backend-oke-api-service.csproj" -c Release -o /app/publish /p:UseAppHost=false

# Runtime stage
FROM mcr.microsoft.com/dotnet/aspnet:9.0 AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENV ASPNETCORE_URLS=http://+:5000
EXPOSE 5000
ENTRYPOINT ["dotnet", "backend-oke-api-service.dll"]

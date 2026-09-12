
# Build stage
# =========================
FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build

WORKDIR /src

# Copy project file first for better Docker layer caching
COPY payment-service.csproj .

# Restore NuGet dependencies
RUN dotnet restore

# Copy application source
COPY Program.cs .

# Build and publish
RUN dotnet publish payment-service.csproj \
    -c Release \
    -o /app/publish \
    --no-restore


# =========================
# Runtime stage
# =========================
FROM mcr.microsoft.com/dotnet/aspnet:8.0

WORKDIR /app

# Copy published application
COPY --from=build /app/publish .

EXPOSE 8085

# Run as non-root
USER $APP_UID

ENTRYPOINT ["dotnet", "payment-service.dll"]

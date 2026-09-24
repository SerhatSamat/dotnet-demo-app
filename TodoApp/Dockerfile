FROM mcr.microsoft.com/dotnet/sdk:10.0 AS build
WORKDIR /src

COPY . .

# Voer restore uit met netwerk-fallback
RUN dotnet restore TodoApp.csproj --disable-parallel || dotnet restore TodoApp.csproj

# Test-stage
FROM build AS testrunner
WORKDIR /src/TodoApp.Tests
ENTRYPOINT ["dotnet", "test"]

# Publish-stage
FROM build AS publish
WORKDIR /src
RUN dotnet publish TodoApp.csproj -c Release -o /app --no-restore

# Runtime-stage
FROM mcr.microsoft.com/dotnet/aspnet:10.0 AS final
WORKDIR /app
COPY --from=publish /app .
ENTRYPOINT ["dotnet", "TodoApp.dll"]
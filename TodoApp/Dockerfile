# 1. Build & Test stage
FROM mcr.microsoft.com/dotnet/sdk:10.0 AS build
WORKDIR /src

# Kopieer alle bestanden
COPY . .

# Restaureer en bouw de oplossing
RUN dotnet restore TodoApp.csproj

# Test runner stage
FROM build AS testrunner
WORKDIR /src/TodoApp.Tests
CMD ["dotnet", "test"]

# Publish stage
FROM build AS publish
WORKDIR /src
RUN dotnet publish TodoApp.csproj -c Release -o /app

# 2. Runtime stage
FROM mcr.microsoft.com/dotnet/aspnet:10.0 AS final
WORKDIR /app
COPY --from=publish /app .
ENTRYPOINT ["dotnet", "TodoApp.dll"]
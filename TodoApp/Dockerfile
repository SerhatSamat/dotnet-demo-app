# 1. Build & Test stage (bevat de .NET SDK)
FROM mcr.microsoft.com/dotnet/sdk:10.0 AS build
WORKDIR /src

# Kopieer projectbestanden en herstel dependencies
COPY TodoApp/TodoApp.csproj TodoApp/
COPY TodoApp.Tests/TodoApp.Tests.csproj TodoApp.Tests/
RUN dotnet restore TodoApp/TodoApp.csproj

# Kopieer de rest van de broncode
COPY . .

# Voer de unit tests uit
FROM build AS testrunner
WORKDIR /src/TodoApp.Tests
CMD ["dotnet", "test", "--logger:trx"]

# Publish de applicatie
FROM build AS publish
WORKDIR /src/TodoApp
RUN dotnet publish -c Release -o /app --no-restore

# 2. Runtime stage (lichtgewicht image voor productie/deployment)
FROM mcr.microsoft.com/dotnet/aspnet:10.0 AS final
WORKDIR /app
COPY --from=publish /app .
ENTRYPOINT ["dotnet", "TodoApp.dll"]
# 1. Build & Test stage (bevat de .NET SDK)
FROM mcr.microsoft.com/dotnet/sdk:10.0 AS build
WORKDIR /src

# Kopieer de projectbestanden vanaf de juiste locaties
COPY TodoApp.csproj .
COPY TodoApp.Tests/TodoApp.Tests.csproj TodoApp.Tests/
RUN dotnet restore TodoApp.Tests/TodoApp.Tests.csproj

# Kopieer de rest van de broncode
COPY . .

# Voer de unit tests uit
FROM build AS testrunner
WORKDIR /src/TodoApp.Tests
CMD ["dotnet", "test", "--logger:trx"]

# Publish de hoofdapplicatie
FROM build AS publish
WORKDIR /src
RUN dotnet publish TodoApp.csproj -c Release -o /app --no-restore

# 2. Runtime stage (voor productie/deployment)
FROM mcr.microsoft.com/dotnet/aspnet:10.0 AS final
WORKDIR /app
COPY --from=publish /app .
ENTRYPOINT ["dotnet", "TodoApp.dll"]
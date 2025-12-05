# === Etapa Base (Ejecución) ===
FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
WORKDIR /app
EXPOSE 10000

# === Etapa Build (Compilación) ===
FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
WORKDIR /src
# Copia la solución completa (todos los archivos y subcarpetas)
COPY . .

# Restaura las dependencias de toda la solución
RUN dotnet restore "Zoologico.Modelos.sln"

# Construye el proyecto API específico
RUN dotnet build "Zoologico.API/Zoologico.API.csproj" -c Release -o /app/build

# === Etapa Publish ===
FROM build AS publish
# Publica el proyecto API específico
RUN dotnet publish "Zoologico.API/Zoologico.API.csproj" -c Release -o /app/publish

# === Etapa Final ===
FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Zoologico.API.dll"]
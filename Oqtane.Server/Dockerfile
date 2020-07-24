#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/core/aspnet:3.1-buster-slim AS base
WORKDIR /app
EXPOSE 80

FROM mcr.microsoft.com/dotnet/core/sdk:3.1-buster AS build
WORKDIR /src
COPY ["Oqtane.Server/Oqtane.Server.csproj", "Oqtane.Server/"]
COPY ["Oqtane.Shared/Oqtane.Shared.csproj", "Oqtane.Shared/"]
COPY ["Oqtane.Client/Oqtane.Client.csproj", "Oqtane.Client/"]
RUN dotnet restore "Oqtane.Server/Oqtane.Server.csproj"
COPY . .
WORKDIR "/src/Oqtane.Server"
RUN dotnet build "Oqtane.Server.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "Oqtane.Server.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Oqtane.Server.dll"]

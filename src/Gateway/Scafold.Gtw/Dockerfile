#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
WORKDIR /src
COPY ["src/Gateway/Scafold.Gtw/Scafold.Gtw.csproj", "src/Gateway/Scafold.Gtw/"]
RUN dotnet restore "src/Gateway/Scafold.Gtw/Scafold.Gtw.csproj"
COPY . .
WORKDIR "/src/src/Gateway/Scafold.Gtw"
RUN dotnet build "Scafold.Gtw.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "Scafold.Gtw.csproj" -c Release -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Scafold.Gtw.dll"]
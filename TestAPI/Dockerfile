#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
WORKDIR /app
# EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
WORKDIR /src
COPY ["TestAPI2/TestAPI2.csproj", "TestAPI/"]
RUN dotnet restore "TestAPI/TestAPI2.csproj" -r linux-arm64
COPY . .
WORKDIR "/src/TestAPI2"
RUN dotnet build "TestAPI2.csproj" -c Release -o /app/build 

FROM build AS publish
RUN dotnet publish "TestAPI2.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app

COPY --from=publish /app/publish .
ENV ASPNETCORE_URLS http://*:5001
ENTRYPOINT ["dotnet", "TestAPI2.dll"]
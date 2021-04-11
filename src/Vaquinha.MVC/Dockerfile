#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:3.1 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:3.1 AS build
WORKDIR /src
COPY ["src/Vaquinha.MVC/Vaquinha.MVC.csproj", "src/Vaquinha.MVC/"]
COPY ["src/Vaquinha.Service/Vaquinha.Service.csproj", "src/Vaquinha.Service/"]
COPY ["src/Vaquinha.Repository/Vaquinha.Repository.csproj", "src/Vaquinha.Repository/"]
COPY ["src/Vaquinha.Domain/Vaquinha.Domain.csproj", "src/Vaquinha.Domain/"]
RUN dotnet restore "src/Vaquinha.MVC/Vaquinha.MVC.csproj"
COPY . .
WORKDIR "/src/src/Vaquinha.MVC"
RUN dotnet build "Vaquinha.MVC.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "Vaquinha.MVC.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Vaquinha.MVC.dll"]
FROM microsoft/dotnet:2.1-aspnetcore-runtime AS base
WORKDIR /app
#EXPOSE 5000

#FROM microsoft/dotnet:2.1-sdk AS build
#WORKDIR /src
#COPY ["MsSqlCodeDiffVersioning/MsSqlCodeDiffVersioning.WebApplication.csproj", "MsSqlCodeDiffVersioning/"]
#COPY ["CommonUtilities.NET.Core.2.x/CommonUtilities.NET.Core.2.x.csproj", "CommonUtilities.NET.Core.2.x/"]
#RUN dotnet restore "MsSqlCodeDiffVersioning/MsSqlCodeDiffVersioning.WebApplication.csproj"
#COPY . .
#WORKDIR "/src/MsSqlCodeDiffVersioning"
#RUN dotnet build "MsSqlCodeDiffVersioning.WebApplication.csproj" -c Release -o /app

#FROM build AS publish
#RUN dotnet publish "MsSqlCodeDiffVersioning.WebApplication.csproj" -c Release -o /app

FROM base AS final
WORKDIR /app
#COPY --from=publish /app .
#COPY ["CommonUtilities.NET.Core.2.x/CommonUtilities.NET.Core.2.x.csproj", "CommonUtilities.NET.Core.2.x/"]
ADD ["bin/publish/Linux-x64/netcoreapp2.1/", "./"]

ENTRYPOINT ["dotnet", "MsSqlCodeDiffVersioning.WebApplication.dll"]
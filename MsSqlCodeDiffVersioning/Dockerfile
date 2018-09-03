FROM microsoft/dotnet:2.1-aspnetcore-runtime AS base
WORKDIR /app
EXPOSE 5000

FROM microsoft/dotnet:2.1-sdk AS build
WORKDIR /src
COPY ["MsSqlCodeDiffVersioning/MsSqlCodeDiffVersioning.WebApplication.csproj", "MsSqlCodeDiffVersioning/"]
COPY ["CommonUtilities.NET.Core.2.x/CommonUtilities.NET.Core.2.x.csproj", "CommonUtilities.NET.Core.2.x/"]
COPY ["MsSqlCodeDiffVersioning/StoreProcedureWebApiExecutorsPlugins/MsSQL.StoreProcedureWebApiExecutor.Plugin/MsSQL.StoreProcedureWebApiExecutor.Plugin.csproj", "StoreProcedureWebApiExecutorsPlugins/MsSQL.StoreProcedureWebApiExecutor.Plugin/"]
COPY ["MsSqlCodeDiffVersioning/StoreProcedureWebApiExecutorsPlugins/MySQL.StoreProcedureWebApiExecutor.Plugin/MySQL.StoreProcedureWebApiExecutor.Plugin.csproj", "StoreProcedureWebApiExecutorsPlugins/MySQL.StoreProcedureWebApiExecutor.Plugin/"]
RUN dotnet restore "MsSqlCodeDiffVersioning/MsSqlCodeDiffVersioning.WebApplication.csproj"
RUN dotnet restore "StoreProcedureWebApiExecutorsPlugins/MsSQL.StoreProcedureWebApiExecutor.Plugin/MsSQL.StoreProcedureWebApiExecutor.Plugin.csproj"
RUN dotnet restore "StoreProcedureWebApiExecutorsPlugins/MySQL.StoreProcedureWebApiExecutor.Plugin/MySQL.StoreProcedureWebApiExecutor.Plugin.csproj"
COPY . .
WORKDIR "/src/MsSqlCodeDiffVersioning"
RUN dotnet build "MsSqlCodeDiffVersioning.WebApplication.csproj" -c Debug -o /app
RUN dotnet build "StoreProcedureWebApiExecutorsPlugins/MsSQL.StoreProcedureWebApiExecutor.Plugin/MsSQL.StoreProcedureWebApiExecutor.Plugin.csproj" -c Debug -o /app/CompositionPlugins
RUN dotnet build "StoreProcedureWebApiExecutorsPlugins/MySQL.StoreProcedureWebApiExecutor.Plugin/MySQL.StoreProcedureWebApiExecutor.Plugin.csproj" -c Debug -o /app/CompositionPlugins

FROM build AS publish
RUN dotnet publish "MsSqlCodeDiffVersioning.WebApplication.csproj" -c Debug -o /app

FROM base AS final
WORKDIR /app
COPY --from=publish /app .
ENTRYPOINT ["dotnet", "MsSqlCodeDiffVersioning.WebApplication.dll"]
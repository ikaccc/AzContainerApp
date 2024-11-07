FROM mcr.microsoft.com/dotnet/runtime:8.0 AS base
USER app
WORKDIR /app
ENV STATUS_ENDPOINT_URI=http://localhost:5113/status

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["POC.Worker.csproj", ""]
RUN dotnet restore "POC.Worker.csproj"
COPY . .
WORKDIR "/src"
RUN dotnet build "POC.Worker.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "POC.Worker.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "POC.Worker.dll"]
# Use the official ASP.NET Core runtime as a base image
FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
WORKDIR /app
EXPOSE 80

# Use the SDK image to build the project
FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
WORKDIR /src
COPY commandcontrolserver.api/CommandControlServer.Api.csproj commandcontrolserver.api/
RUN dotnet restore "commandcontrolserver.api/CommandControlServer.Api.csproj"
COPY . .
WORKDIR "/src/commandcontrolserver.api"
RUN dotnet build "CommandControlServer.Api.csproj" -c Release -o /app/build

# Publish the project
FROM build AS publish
RUN dotnet publish "CommandControlServer.Api.csproj" -c Release -o /app/publish

# Final stage: copy the build output to the runtime image
FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "CommandControlServer.Api.dll"]

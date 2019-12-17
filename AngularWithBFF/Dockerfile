#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/core/aspnet:3.1-buster-slim AS base
# install node onto image
RUN curl -sL https://deb.nodesource.com/setup_10.x | bash -
RUN apt-get install --assume-yes nodejs
RUN npm install
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/core/sdk:3.1-buster AS build
RUN curl -sL https://deb.nodesource.com/setup_10.x | bash -
RUN apt-get install --assume-yes nodejs
RUN npm install
WORKDIR /src
COPY ["AngularWithBFF/AngularWithBFF.csproj", "AngularWithBFF/"]
RUN dotnet restore "AngularWithBFF/AngularWithBFF.csproj"
COPY . .
WORKDIR "/src/AngularWithBFF"
RUN dotnet build "AngularWithBFF.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "AngularWithBFF.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "AngularWithBFF.dll"]

FROM mcr.microsoft.com/dotnet/core/sdk:3.1-buster AS build


RUN curl --silent --location https://deb.nodesource.com/setup_10.x | bash -
RUN apt-get install --yes nodejs



WORKDIR /

WORKDIR /src
COPY . .


RUN dotnet restore "./Rozklad.V2.csproj"

WORKDIR /ClientApp
RUN npm install
WORKDIR /src

RUN ls
RUN dotnet publish "Rozklad.V2.csproj" -c Release -o /app/publish


FROM mcr.microsoft.com/dotnet/core/aspnet:3.1-buster-slim


COPY --from=trajano/ubuntu-faketime  /faketime.so /lib/faketime.so
ENV LD_PRELOAD=/lib/faketime.so

EXPOSE 80


COPY --from=build /app/publish .


ENTRYPOINT ["dotnet", "Rozklad.V2.dll"]
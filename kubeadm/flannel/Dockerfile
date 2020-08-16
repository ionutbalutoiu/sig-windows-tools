ARG servercoreTag="ltsc2019"
ARG golangTag=windowsservercore-1809

FROM golang:${golangTag} as builder
ADD setup.go build/
RUN go build -o build/setup.exe build/setup.go

FROM mcr.microsoft.com/windows/servercore:${servercoreTag}
SHELL ["powershell", "-NoLogo", "-Command", "$ErrorActionPreference = 'Stop'; $ProgressPreference = 'SilentlyContinue';"]

RUN mkdir -force C:\k\flannel; \
  pushd C:\k\flannel; \
  curl.exe -LO https://github.com/coreos/flannel/releases/download/v0.12.0/flanneld.exe

ADD hns.psm1 /k/flannel
COPY --from=builder /gopath/build/setup.exe /k/flannel/setup.exe

RUN mkdir C:\utils; \
  curl.exe -Lo C:\utils\wins.exe https://github.com/rancher/wins/releases/download/v0.0.4/wins.exe; \
  curl.exe -Lo C:\utils\yq.exe https://github.com/mikefarah/yq/releases/download/2.4.1/yq_windows_amd64.exe; \
  "[Environment]::SetEnvironmentVariable('PATH', $env:PATH + ';C:\utils', [EnvironmentVariableTarget]::Machine)"

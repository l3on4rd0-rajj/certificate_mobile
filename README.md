param(
    [string]$network,
    [int]$cidr
)

# Nome do arquivo de saída
$output = "ativos.txt"

# Limpar arquivo, se existir
if (Test-Path $output) {
    Clear-Content $output
}

# Calcular IP base
$hosts = [math]::Pow(2, 32 - $cidr) - 2
$ipBytes = $network.Split('.') | ForEach-Object { [int]$_ }
$baseAddress = ($ipBytes[0] -shl 24) -bor ($ipBytes[1] -shl 16) -bor ($ipBytes[2] -shl 8) -bor $ipBytes[3]

for ($i = 1; $i -le $hosts; $i++) {
    $currentIPInt = $baseAddress + $i
    $b1 = ($currentIPInt -shr 24) -band 255
    $b2 = ($currentIPInt -shr 16) -band 255
    $b3 = ($currentIPInt -shr 8) -band 255
    $b4 = $currentIPInt -band 255
    $ip = "$b1.$b2.$b3.$b4"

    Write-Host "Testando $ip"
    if (Test-Connection -ComputerName $ip -Count 1 -Quiet) {
        Write-Host "$ip está ativo!" -ForegroundColor Green
        Add-Content -Path $output -Value $ip
    }
}
----

@echo off
setlocal

if "%~1"=="" (
    echo Uso: pingar_subrede.bat [IP_BASE] [CIDR]
    echo Exemplo: pingar_subrede.bat 192.168.1.0 24
    exit /b
)

if "%~2"=="" (
    echo ERRO: Voce deve fornecer o CIDR (ex: 24).
    exit /b
)

set IP_REDE=%~1
set CIDR=%~2

echo Executando varredura em %IP_REDE%/%CIDR% ...
powershell -ExecutionPolicy Bypass -File "%~dp0ping_subnet.ps1" -network "%IP_REDE%" -cidr %CIDR%

pause

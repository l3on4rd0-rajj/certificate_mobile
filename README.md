function Ping-Subnet {
    param(
        [string]$network,
        [int]$cidr
    )

    # Converter CIDR em máscara decimal e número de hosts
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
        }
    }
}


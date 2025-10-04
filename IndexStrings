$FilePath = ".\strings.txt"
$Results = @()

foreach ($Line in Get-Content $FilePath) {
    $Signature = Get-AuthenticodeSignature $Line -ErrorAction SilentlyContinue
    $SignatureStatus = "No signature found"

    if ($Signature.Status -eq "Valid") {
        $SignatureStatus = "Valid"
    }
    elseif ($Signature.Status -eq "NotSigned") {
        $SignatureStatus = "No signature found"
    }
    elseif ($Signature.Status -eq "UnknownError") {
        $SignatureStatus = "Unknown error"
    }
    elseif ($Signature.Status -eq "HashMismatch") {
        $SignatureStatus = "File has been modified"
    }
    elseif ($Signature.Status -eq "NotTrusted") {
        $SignatureStatus = "Certificate not trusted"
    }

    $FakeSignature = $false
    if (($SignatureStatus -ne "Valid") -and ($SignatureStatus -ne "No signature found")) {
        $FakeSignature = $true
    }

    $Results += [PSCustomObject]@{
        FilePath = $Line
        SignatureStatus = $SignatureStatus
        FakeSignature = $FakeSignature
    }
}

$Results | Format-Table -AutoSize

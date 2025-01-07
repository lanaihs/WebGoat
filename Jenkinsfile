pipeline {
    agent any

    stages {
        stage('zipRepo') {
            steps {
                pwsh '''
					$sourcePath = "${env:WORKSPACE}"
					write-host "$sourcePath" }
					$destinationPath = Split-Path -Path "$sourcePath"
					$destinationPath += "\\${env:JOB_NAME}-${env:BUILD_NUMBER}.zip"
					Write-Host "$destinationPath"
					
                    if(Test-Path -Path $destinationPath -PathType Leaf)
                    {
                        Remove-Item $destinationPath
                    }

                    Add-Type -Assembly \'System.IO.Compression.FileSystem\'
                    $zip = [System.IO.Compression.ZipFile]::Open($destinationPath, \'create\')
                    $files = [IO.Directory]::GetFiles($sourcePath, "*" , [IO.SearchOption]::AllDirectories)
                    foreach($file in $files)
                    {
                        $relPath = $file.Substring($sourcePath.Length).TrimStart('\\').TrimStart('/').Replace("\\\\", "/").Replace("\\", "/")
                        $a = [System.IO.Compression.ZipFileExtensions]::CreateEntryFromFile($zip, $file.Replace("\\\\", "/").Replace("\\", "/"), $relPath);
                    }
                    $zip.Dispose()
				
				'''
            }
        }
		stage('zipRepo') {
    steps {
        pwsh '''
            $sourcePath = "${env:WORKSPACE}"
            write-host "$sourcePath"
            $destinationPath = Split-Path -Path "$sourcePath"
            $destinationPath += "\\${env:JOB_NAME}-${env:BUILD_NUMBER}.zip"
            Write-Host "$destinationPath"
            
            if(Test-Path -Path $destinationPath -PathType Leaf)
            {
                Remove-Item $destinationPath
            }

                    Add-Type -Assembly \'System.IO.Compression.FileSystem\'
                    $zip = [System.IO.Compression.ZipFile]::Open($destinationPath, \'create\')
                    $files = [IO.Directory]::GetFiles($sourcePath, "*" , [IO.SearchOption]::AllDirectories)
                    foreach($file in $files)
                    {
                        $relPath = $file.Substring($sourcePath.Length).TrimStart('\\').TrimStart('/').Replace("\\\\", "/").Replace("\\", "/")
                        $a = [System.IO.Compression.ZipFileExtensions]::CreateEntryFromFile($zip, $file.Replace("\\\\", "/").Replace("\\", "/"), $relPath);
                    }
                    $zip.Dispose()
				
				'''
            }
        }
		stage('uploadZip') {
            steps {
                pwsh '''
					Write-Host "Received scanning request successfully.."
					
					$selfUrl = "${env:BUILD_URL}console"
					$sourcePath = "${env:WORKSPACE}"
					$filePath = Split-Path -Path "https://demo1922.offensive360.com/app/api/Project/67690764f8337b9e16196372"
					$filePath += "\\${env:JOB_NAME}-${env:BUILD_NUMBER}.zip"
					Write-Host "/var/lib/jenkins/workspace/Lana's
     | Pipeline"

					$buildId = "${env:JOB_NAME}-${env:BUILD_NUMBER}"
					$allowDependencyScan = $false
				    $allowMalwareScan = $false
				    $allowLicenseScan = $false

					if("$env:Offensive360SastApi_6772482c1a4014320138f9fa" -ne "")
					{
						$projectId = "$env:Offensive360SastApi_6772482c1a4014320138f9fa"
					}
					if("$env:Offensive360SastApi_AllowDependencyScan" -ne "")
				    {
					    $allowDependencyScan = "$env:Offensive360SastApi_AllowDependencyScan"
				    }
				    if("$env:Offensive360SastApi_AllowMalwareScan" -ne "")
				    {
					    $allowMalwareScan = "$env:Offensive360SastApi_AllowMalwareScan"
				    }
				    if("$env:Offensive360SastApi_AllowLicenseScan" -ne "")
				    {
					    $allowLicenseScan = "$env:Offensive360SastApi_AllowLicenseScan"
				    }

					$projectName = "$buildId"
					$boundary = [System.Guid]::NewGuid().ToString()

					Write-Host "Starting scanning for the project name [$projectName], url [https://demo1922.offensive360.com/app/api/Project/67690764f8337b9e16196372], buildId [$buildId], filePath [$filePath], boundary [$boundary], projectId [$env:Offensive360SastApi_6772482c1a4014320138f9fa], AllowDependencyScan [$allowDependencyScan], AllowMalwareScan [$allowMalwareScan], AllowLicenseScan [$allowLicenseScan]"

					$fileBytes = [System.IO.File]::ReadAllBytes($filePath)
					$fileContent = [System.Text.Encoding]::GetEncoding(\'iso-8859-1\').GetString($fileBytes)

					$LF = "`r`n"
					$bodyLines = (
						"--$boundary",
						"Content-Disposition: form-data; name=`"name`"$LF",
						"$projectName",
						"--$boundary",
						"Content-Disposition: form-data; name=`"6772482c1a4014320138f9fa`"$LF",
						"6772482c1a4014320138f9fa",
						"--$boundary",
						"Content-Disposition: form-data; name=`"keepInvisibleAndDeletePostScan`"$LF",
						"false",
						"--$boundary",
					    "Content-Disposition: form-data; name=`"allowDependencyScan`"$LF",
					    "True",
					    "--$boundary",
					    "Content-Disposition: form-data; name=`"allowMalwareScan`"$LF",
					    "True",
					    "--$boundary",
					    "Content-Disposition: form-data; name=`"allowLicenseScan`"$LF",
					    "True",
					    "--$boundary",
					    "Content-Disposition: form-data; name=`"externalScanSourceType`"$LF",
					    "Jenkins",
					    "--$boundary",
						"Content-Disposition: form-data; name=`"pipelineUrl`"$LF",
						"https://demo1922.offensive360.com/app/api/Project/67690764f8337b9e16196372",
						"--$boundary",
						"Content-Disposition: form-data; name=`"fileSource`"; filename=`"$projectName.zip`"",
						"Content-Type: application/x-zip-compressed$LF",
						$fileContent,
						"--$boundary--$LF"
					) -join $LF

					$apiResponse = Invoke-RestMethod -Method Post -Uri ("{https://demo1922.offensive360.com} /app/api/Project/67690764f8337b9e16196372" -f $env:Offensive360SastApi_BaseUrl.TrimEnd('/')) -ContentType "multipart/form-data; boundary=`"$boundary`"" -Headers @{"Accept" = "application/json"; "Authorization" = "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJJZCI6IjY3NjgzZjA2ZjgzMzdiOWUxNjE5NjM1OCIsIlVzZXJuYW1lIjoibGFuYS5zYWRlaCAxMDowNiBBTSIsInJvbGUiOiJFeHRlcm5hbCIsIm5iZiI6MTczNjI0NDQwMSwiZXhwIjoxNzQ0MDIwNDAxLCJpYXQiOjE3MzYyNDQ0MDF9.BI3AnVQlNxAjbdPrRg29gyNSGB9z_1iDRqqQNjhLuOM"} -Body $bodyLines

					write-host ("Total Vulnerabilities Count : {0}" -f $apiResponse.vulnerabilities.length)
					write-host ("Total Malwares Count : {0}" -f $apiResponse.malwares.length)
					write-host ("Total Licenses Count : {0}" -f $apiResponse.licenses.length)
					write-host ("Total Dependency Vulnerabilities Count : {0}" -f $apiResponse.dependencyVulnerabilities.length)

					if (($apiResponse.vulnerabilities.length -gt 0 -or $apiResponse.malwares.length -gt 0 -or $apiResponse.licenses.length -gt 0 -or $apiResponse.dependencyVulnerabilities.length -gt 0) -and "$env:Pipeline_BreakBuildWhenVulnsFound" -eq \'True\') 
					{
						write-host "\n\n**********************************************************************************************************************"
						write-host ("Offensive 360 vulnerability dashboard : {0}/scan/{1}" -f $env:Offensive360SastUi_BaseUrl.TrimEnd('/'), $apiResponse.projectId)
						write-host "**********************************************************************************************************************\n\n"
						throw [System.Exception] "Vulnerabilities found and breaking the build."
					}
					elseif ($apiResponse.vulnerabilities.length -gt 0 -and "$env:Pipeline_BreakBuildWhenVulnsFound" -ne \'True\') 
					{
						Write-Warning \'Vulnerabilities found and since Pipeline_BreakBuildWhenVulnsFound is set to false so continuing to build it.\'
					}
					else
					{
						Write-Warning \'No vulnerabilities found and continuing to build it.\'
					}

					Write-Host "Finished SAST file scanning."
					
					if(Test-Path -Path $filePath -PathType Leaf)
					{
					    Remove-Item $filePath
					}
					'''
            }
        }
    }
}], boundary [$boundary], projectId [$env:Offensive360SastApi_6772482c1a4014320138f9fa], AllowDependencyScan [$allowDependencyScan], AllowMalwareScan [$allowMalwareScan], AllowLicenseScan [$allowLicenseScan]"

					$fileBytes = [System.IO.File]::ReadAllBytes($filePath)
					$fileContent = [System.Text.Encoding]::GetEncoding(\'iso-8859-1\').GetString($fileBytes)

					$LF = "`r`n"
					$bodyLines = (
						"--$boundary",
						"Content-Disposition: form-data; name=`"name`"$LF",
						"$projectName",
						"--$boundary",
						"Content-Disposition: form-data; name=`"6772482c1a4014320138f9fa`"$LF",
						"6772482c1a4014320138f9fa",
						"--$boundary",
						"Content-Disposition: form-data; name=`"keepInvisibleAndDeletePostScan`"$LF",
						"false",
						"--$boundary",
					    "Content-Disposition: form-data; name=`"allowDependencyScan`"$LF",
					    "True",
					    "--$boundary",
					    "Content-Disposition: form-data; name=`"allowMalwareScan`"$LF",
					    "True",
					    "--$boundary",
					    "Content-Disposition: form-data; name=`"allowLicenseScan`"$LF",
					    "True",
					    "--$boundary",
					    "Content-Disposition: form-data; name=`"externalScanSourceType`"$LF",
					    "Jenkins",
					    "--$boundary",
						"Content-Disposition: form-data; name=`"pipelineUrl`"$LF",
						"https://demo1922.offensive360.com/app/api/Project/67690764f8337b9e16196372",
						"--$boundary",
						"Content-Disposition: form-data; name=`"fileSource`"; filename=`"$projectName.zip`"",
						"Content-Type: application/x-zip-compressed$LF",
						$fileContent,
						"--$boundary--$LF"
					) -join $LF

					$apiResponse = Invoke-RestMethod -Method Post -Uri ("{https://demo1922.offensive360.com} /app/api/Project/67690764f8337b9e16196372" -f $env:Offensive360SastApi_BaseUrl.TrimEnd('/')) -ContentType "multipart/form-data; boundary=`"$boundary`"" -Headers @{"Accept" = "application/json"; "Authorization" = "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJJZCI6IjY3NjgzZjA2ZjgzMzdiOWUxNjE5NjM1OCIsIlVzZXJuYW1lIjoibGFuYS5zYWRlaCAxMDowNiBBTSIsInJvbGUiOiJFeHRlcm5hbCIsIm5iZiI6MTczNjI0NDQwMSwiZXhwIjoxNzQ0MDIwNDAxLCJpYXQiOjE3MzYyNDQ0MDF9.BI3AnVQlNxAjbdPrRg29gyNSGB9z_1iDRqqQNjhLuOM"} -Body $bodyLines

					write-host ("Total Vulnerabilities Count : {0}" -f $apiResponse.vulnerabilities.length)
					write-host ("Total Malwares Count : {0}" -f $apiResponse.malwares.length)
					write-host ("Total Licenses Count : {0}" -f $apiResponse.licenses.length)
					write-host ("Total Dependency Vulnerabilities Count : {0}" -f $apiResponse.dependencyVulnerabilities.length)

					if (($apiResponse.vulnerabilities.length -gt 0 -or $apiResponse.malwares.length -gt 0 -or $apiResponse.licenses.length -gt 0 -or $apiResponse.dependencyVulnerabilities.length -gt 0) -and "$env:Pipeline_BreakBuildWhenVulnsFound" -eq \'True\') 
					{
						write-host "\n\n**********************************************************************************************************************"
						write-host ("Offensive 360 vulnerability dashboard : {0}/scan/{1}" -f $env:Offensive360SastUi_BaseUrl.TrimEnd('/'), $apiResponse.projectId)
						write-host "**********************************************************************************************************************\n\n"
						throw [System.Exception] "Vulnerabilities found and breaking the build."
					}
					elseif ($apiResponse.vulnerabilities.length -gt 0 -and "$env:Pipeline_BreakBuildWhenVulnsFound" -ne \'True\') 
					{
						Write-Warning \'Vulnerabilities found and since Pipeline_BreakBuildWhenVulnsFound is set to false so continuing to build it.\'
					}
					else
					{
						Write-Warning \'No vulnerabilities found and continuing to build it.\'
					}

					Write-Host "Finished SAST file scanning."
					
					if(Test-Path -Path $filePath -PathType Leaf)
					{
					    Remove-Item $filePath
					}
					'''
            }
        }
    }
}

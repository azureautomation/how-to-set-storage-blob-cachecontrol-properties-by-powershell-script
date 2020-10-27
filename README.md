How to set storage blob cache-control Properties by PowerShell script
=====================================================================

            




How to set storage blob cache-control
Properties by PowerShell script


Introduction


 


Setting Cache-Control on
Azure Blobs can help reduce bandwidth
and improve the performance by preventing consumers from having to continuously download resources.



Cache-Control allows you to specify a relative amount of time to cache data after it was received. It’s mostly recommended when you need control over how caching is done.


Usually, if we wanted to set the cache-control property of blob files, we can do such work through azure portal/windows azure storage explore… However, if you want to set the cache-control property of every file in your blob, using such ways
 will cost lots of time, and you may also need to change this property overtimes. A better way to do this is creating
C# program using Azure Storage Client assembly and set it.


However, this blog will consider to use PowerShell script to do same work because it’s more convenient to change the
property when we want. This example is to help us set storage blob cache-control
Properties by PowerShell script, one thing should be noticed is we still need to use
“Microsoft.WindowsAzure.StorageClient.dll” to access the blobs.


 


**Note: **please download latest version sample [here](https://github.com/Azure-Samples/storage-blobs-powershell-cache-control/archive/master.zip). 


Example


  



PowerShell
Edit|Remove
powershell
   #set Properties

   $blobRef.Properties.CacheControl = $cacheControlValue

   $blobRef.SetProperties() 

   #set Properties 
 
   $blobRef.Properties.CacheControl = $cacheControlValue 
 
   $blobRef.SetProperties() 




Scenario


You need to set storage blob cache-control property 
/ check cache-control property through PowerShell script easily.



Requirements


PowerShell Version > 3.0


Windows Azure PowerShell > 1.0.0


See Also


 Using
 Azure PowerShell with Azure Storage


Set blob Properties


Script Content


 The content of the script is reproduced below


• Download and install the latest PowerShell, and connect to
you subscription.


  



PowerShell
Edit|Remove
powershell
Import-AzurePublishSettingsFile -Environment AzureChinaCloud E:\PublishSettings\CIETest03-12-9-2015-credentials.publishsettings 

Import-AzurePublishSettingsFile -Environment AzureChinaCloud E:\PublishSettings\CIETest03-12-9-2015-credentials.publishsettings 




      • Set the value of
Properties and Metadata





PowerShell
Edit|Remove
powershell
#connection info

$StorageName='your storage account name'

$StorageKey='storage Key”

$ContainerName='containername'

$BlobUri='https://yourstorageaccountname.blob.core.chinacloudapi.cn/'+$ContainerName

 

#get all the blob under your container

$StorageCtx = New-AzureStorageContext -StorageAccountName $StorageName -StorageAccountKey $StorageKey

$blobs = Get-AzureStorageBlob -Container $ContainerName -Context $StorageCtx

 

#creat CloudBlobClient

Add-Type -Path 'C:\Program Files\Microsoft SDKs\Windows Azure\.NET SDK\v2.3\ref\Microsoft.WindowsAzure.StorageClient.dll'

$storageCredentials = New-Object Microsoft.WindowsAzure.StorageCredentialsAccountAndKey -ArgumentList $StorageName,$StorageKey

$blobClient =   New-Object Microsoft.WindowsAzure.StorageClient.CloudBlobClient($BlobUri,$storageCredentials)

 

#set Properties and Metadata

$cacheControlValue = 'public, max-age=60480'

foreach ($blob in $blobs)

{

   #set Metadata

   $blobRef = $blobClient.GetBlobReference($blob.Name)

   $blobRef.Metadata.Add('abcd','abcd')

   $blobRef.SetMetadata()

  

   #set Properties

   $blobRef.Properties.CacheControl = $cacheControlValue

   $blobRef.SetProperties()

} 

#connection info 
 
$StorageName='your storage account name' 
 
$StorageKey='storage Key” 
 
$ContainerName='containername' 
 
$BlobUri='https://yourstorageaccountname.blob.core.chinacloudapi.cn/'+$ContainerName 
 
  
 
#get all the blob under your container 
 
$StorageCtx = New-AzureStorageContext -StorageAccountName $StorageName -StorageAccountKey $StorageKey 
 
$blobs = Get-AzureStorageBlob -Container $ContainerName -Context $StorageCtx 
 
  
 
#creat CloudBlobClient 
 
Add-Type -Path 'C:\Program Files\Microsoft SDKs\Windows Azure\.NET SDK\v2.3\ref\Microsoft.WindowsAzure.StorageClient.dll' 
 
$storageCredentials = New-Object Microsoft.WindowsAzure.StorageCredentialsAccountAndKey -ArgumentList $StorageName,$StorageKey 
 
$blobClient =   New-Object Microsoft.WindowsAzure.StorageClient.CloudBlobClient($BlobUri,$storageCredentials) 
 
  
 
#set Properties and Metadata 
 
$cacheControlValue = 'public, max-age=60480' 
 
foreach ($blob in $blobs) 
 
{ 
 
   #set Metadata 
 
   $blobRef = $blobClient.GetBlobReference($blob.Name) 
 
   $blobRef.Metadata.Add('abcd','abcd') 
 
   $blobRef.SetMetadata() 
 
   
 
   #set Properties 
 
   $blobRef.Properties.CacheControl = $cacheControlValue 
 
   $blobRef.SetProperties() 
 
} 




• Test result:


• Before run script:


 


![Image](https://github.com/azureautomation/how-to-set-storage-blob-cachecontrol-properties-by-powershell-script/raw/master/image.png)



• Run script:


![Image](https://github.com/azureautomation/how-to-set-storage-blob-cachecontrol-properties-by-powershell-script/raw/master/image.png)



• Result:


![Image](https://github.com/azureautomation/how-to-set-storage-blob-cachecontrol-properties-by-powershell-script/raw/master/image.png)



 




Microsoft All-In-One Script Framework is an automation script sample library for IT Professionals. The key value that All-In-One Script Framework is trying to deliver is Scenario-Focused Script Samples driven by IT
 Pros' real-world pains and needs. The team is monitoring all TechNet forums, IT Pros' support calls to Microsoft, and script requests submitted to TechNet Script Repository. We collect frequently asked IT scenarios, and create script samples to automate the
 tasks and save some time for IT Pros. The team of All-In-One Script Framework sincerely hope that these customer-driven automation script samples can help our IT community in this script-centric move.


        
    
TechNet gallery is retiring! This script was migrated from TechNet script center to GitHub by Microsoft Azure Automation product group. All the Script Center fields like Rating, RatingCount and DownloadCount have been carried over to Github as-is for the migrated scripts only. Note : The Script Center fields will not be applicable for the new repositories created in Github & hence those fields will not show up for new Github repositories.

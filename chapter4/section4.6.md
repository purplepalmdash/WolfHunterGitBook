## Configure CloudStack

### Change Login Password
The first time you login to the installed CloudStack Management node, you will see following webpage:    
![../images/2015_07_22_09_05_39_661x481.jpg](../images/2015_07_22_09_05_39_661x481.jpg)   

Click `Continue with basic installation`, set the password:    
![../images/2015_07_22_09_07_41_289x226.jpg](../images/2015_07_22_09_07_41_289x226.jpg)    

At this point, refresh your browser, and you see the login window again, use your newly modified password for login:    
![../images/2015_07_22_09_08_34_501x250.jpg](../images/2015_07_22_09_08_34_501x250.jpg)  

After login you will see the same webpage which asks you follow with guide or not, we select `I have used CloudStack before, skip this guide`  button and continue with next setup.    

Your webpage listed as following:    
![../images/2015_07_22_09_12_18_629x390.jpg](../images/2015_07_22_09_12_18_629x390.jpg)   

### CloudStack Global Options
Since we want to use local storage, we have to do following configuration for enable Local Storage. 

Hit `Global Options` button and type in `local` for searching:    
![../images/2015_07_22_09_35_07_918x522.jpg](../images/2015_07_22_09_35_07_918x522.jpg)   

Change the value `system.vm.usel` to `true:    
![../images/2015_07_22_09_35_26_701x255.jpg](../images/2015_07_22_09_35_26_701x255.jpg)    

Restart Cloudstack-management service
```
# service cloudstack-management restart
```

### Infrasturcture Configuration
Click `Infrasturcture`, and you will see nothing has been configured yet.    
![../images/2015_07_22_09_13_57_900x469.jpg](../images/2015_07_22_09_13_57_900x469.jpg)    

Click `View All` button under `zone`:   
![../images/2015_07_22_09_15_43_348x186.jpg](../images/2015_07_22_09_15_43_348x186.jpg)

Click `Add zone` button:   
![../images/2015_07_22_09_18_11_704x128.jpg](../images/2015_07_22_09_18_11_704x128.jpg)   

Select Zone Type:   
![../images/2015_07_22_09_19_51_677x560.jpg](../images/2015_07_22_09_19_51_677x560.jpg)    

Configure Zone Info:   
![../images/2015_07_22_09_21_31_641x443.jpg](../images/2015_07_22_09_21_31_641x443.jpg)    

![../images/2015_07_22_09_23_12_482x403.jpg](../images/2015_07_22_09_23_12_482x403.jpg)   

Click Next and click Next again to step `4 Add Resources`, configure the start/end IP Arrange:    
![../images/2015_07_22_09_26_09_672x536.jpg](../images/2015_07_22_09_26_09_672x536.jpg)    

Continue configure Guest Traffic:  
![../images/2015_07_22_09_28_33_672x496.jpg](../images/2015_07_22_09_28_33_672x496.jpg)     

Configure cluster name:   
![../images/2015_07_22_09_30_52_552x245.jpg](../images/2015_07_22_09_30_52_552x245.jpg)    

Add host:   
![../images/2015_07_22_09_32_11_543x378.jpg](../images/2015_07_22_09_32_11_543x378.jpg)   

Add Secondary Storage:   
![../images/2015_07_22_09_33_35_544x348.jpg](../images/2015_07_22_09_33_35_544x348.jpg)   

Hit Launch , and you will see zone/pod created, host has been added into cluster.     
![../images/2015_07_22_11_55_16_687x475.jpg](../images/2015_07_22_11_55_16_687x475.jpg)    

### End Of The Section
Now the cloudstack manually deployment finished, in next section we will discuss on how to use CloudMonkey for automatically create zone/pod/cluster.   

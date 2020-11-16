# list kernel installed free boot space
`yum list installed | grep kernel`

`rpm -qa |grep -i kenel`

Below is output

```
kernel.x86_64                        3.10.0-957.10.1.el7               @updates                                                                                                                                                              
kernel.x86_64                        3.10.0-1062.4.1.el7               @updates                                                                                                                                                              
kernel-tools.x86_64                  3.10.0-1062.4.1.el7               @updates                                                                                                                                                              
kernel-tools-libs.x86_64             3.10.0-1062.4.1.el7               @updates 
```
# remove oldest kernel
`yum remove kernel-3.10.0-957.10.1.el7.x86_64`


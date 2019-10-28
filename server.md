# 1. login VPN server in the lab:

```
ssh -x yujc@111.207.104.144
```

- the password is 

- **DO NOT TELL OTHERS!!!!!!!!!!!!**

# 2. type in VPN server terminal:

```
ssh -x demo@172.16.1.125 -p xxxx
```

- the password is 

- ROS and conda is installed OK.

- the Tello control code is OK, activate the env first.

  - ```
    source activate tellopy
    ```

- xxxx is the port number ,see as follows:

  | zhr  | 2503 |
  | ---- | ---- |
  | xyx  | 2504 |
  | cze  | 2505 |
  | ghq  | 2506 |
  | mzh  | 2507 |
  | dsr  | 2508 |
  | fty  | 2509 |
  | hxy  | 2510 |
  | tcy  | 2511 |
  | syd  | 2512 |
  | zpc  | 2513 |
  | qyj  | 2514 |
  | qyy  | 2515 |
  | tjh  | 2516 |
  | dj   | 2517 |
  | hzy  | 2518 |
  | hsy  | 2519 |
  | jzl  | 2520 |
  | lwx  | 2521 |
  | wzs  | 2522 |
  | zex  | 2523 |
  | wyq  | 2524 |
  | wka  | 2525 |
  | dyc  | 2526 |
  | lzq  | 2527 |
  | lyh  | 2528 |
  | dyf  | 2529 |
  
  **DO NOT USR OTHERS!!!!!!!!!!!!!!!!!**

  - if you want to use the GPU, type this order in the terminal to see whether GPU is available or not. if others is running something in it, you need to wait.

    ```
    nvidia-smi
    ```
  
    only when you see this status, you can run your own code.[346MB << 7981MB means you have enough memory to train your network]
  
    ![img]( https://github.com/zoeyuchao/thudrone/blob/master/nvidia.jpg )
    
    

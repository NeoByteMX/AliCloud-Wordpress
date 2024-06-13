# AliCloud-Wordpress
Tutorial about how to create your WordPress site on Alibaba Cloud using their free trial.

* The first think you need of course is an account, create your and then follow me.

## Creating the free trial.

Once you have created your account and added a card, is time to use the free trial. Go to https://www.alibabacloud.com/en/free?_p_lc=1&TargetUserType=for-enterprise

And select the ECS t5 Instance (1C1G 1 Year).

![image](https://github.com/NeoByteMX/AliCloud-Wordpress/assets/86810793/b62bc5d5-7a6c-4a50-b37b-2d2508313ea9)


1. Select your region, I'll choose US(Silicon Valley)

![image](https://github.com/NeoByteMX/AliCloud-Wordpress/assets/86810793/81d3a864-b273-4ab3-adfc-6bc28b867e9f)

2. Select your OS, I'll choose Ubuntu as is the most popular

![image](https://github.com/NeoByteMX/AliCloud-Wordpress/assets/86810793/c3f34454-1c21-4805-bdbc-2811901ff7b3)

3. You can select your Customer Password or use a Key Pair, I'll use a custom password as is beginner-friendly.

![image](https://github.com/NeoByteMX/AliCloud-Wordpress/assets/86810793/763b7344-612d-494b-9787-a2907a9695fd)

Let the other things as default (or you can't use the free trial)

Then select the box ECS Terms of Service | Product Terms of Service and create order.

## Login in through SSH
After creating your ECS free trial, go to your Console.

![image](https://github.com/NeoByteMX/AliCloud-Wordpress/assets/86810793/3ad9587a-d6ec-4c9b-bee0-3a67a8d057ff)
![image](https://github.com/NeoByteMX/AliCloud-Wordpress/assets/86810793/f8db298c-6ae3-476c-aeae-d81df57f67ab)

And go to ECS

![image](https://github.com/NeoByteMX/AliCloud-Wordpress/assets/86810793/307b7211-ef22-4be6-a3be-ee7e3c83f723)

This is the direct link: https://ecs.console.aliyun.com/home

We can see one running ECS instance in the region which you set, in my case US (silicon Valley)



Select "instances" in the left menu bar:

![image](https://github.com/NeoByteMX/AliCloud-Wordpress/assets/86810793/9aeceafa-c51d-4208-9d33-0a8fa160db0a)

You will see again your instance (check your region)

![image](https://github.com/NeoByteMX/AliCloud-Wordpress/assets/86810793/8d50a487-a14e-48b8-b0f2-3fc527858d4a)

You can see your instance Public IP (I'm using and EIP but doesn't matter)

![image](https://github.com/NeoByteMX/AliCloud-Wordpress/assets/86810793/e2e3a9db-02f0-47b1-89d5-e3f0847c2aba)

#### SSH
The command you will need is `SSH root@YOUR_IP`, once you enter this it will ask you for your fingerprint, type yes, and then type your password (that you use to create your ECS).


I hope this quick tutorial helps you!

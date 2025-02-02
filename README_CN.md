# ComfyUI-LMCQ
**切换英文版: [English](README.md)**
## 介绍

ComfyUI小节点工具包，本工具包主要是更新一些实用的小节点，为comfyui生态做一份贡献，
PS：“LMCQ”是团队名称的简写


## Image

1. LmcqImageSaver：主要对生成出来的图像文件提供水印和是否保存元数据信息功能
2. LmcqImageSaverTransit：功能上与LmcqImageSaver没有太大差别，唯一的区别就是可以将处理过后的图像文件发送到下一个节点
3. LmcqImageSaverWeb：在LmcqImageSaver基础上添加主动发送文件及prompt_id到指定接口地址（设计它的目的是为了不用WS去保持大量的长连接请求）

![img.png](img.png)

~~~
功能详解

filename_prefix：    文件名前缀
format：             文件格式，目前支持‘png、jpg、webp’三种
quality：            文件压缩率，数值越小，压缩率越高，对应的文件大小越小，如果要使用该功能请在文件格式中选择jpg格式
apply_watermark：    是否开启水印功能
watermark_text：     水印文本内容
watermark_size：     水印大小，默认15，最高70
watermark_position： 水印在图片当中的位置
enhance_brightness： 设置图片的明暗程度，数值越低，图片越暗
enhance_contrast：   设置图片的色彩饱和程度，数值越低，饱和度越低
save_metadata：      是否保存图片背后的工作流信息
enable_api_call：    是否开启接口请求，若开启，则会在工作流完成之后将文件及api当中的prompt_id发送到对应接口
api_url：            请求的接口地址

~~~
## flux

1. 由于lllyasviel大佬更新了flux的NF4版本，但comfyui当中没有对应的加载其模型的节点，常规的checkpointLoader无法适配该模型，所以设计该节点用于方便模型使用,使用方式跟常规checkpointLoader一样

![img_1.png](img_1.png)


## 更新日志 2024-08-20

Image系列功能新增
~~~
watermark_type：水印类型，默认文本，可选项：文本、图片

watermark_image：连接你要用于作为水印的图片

watermark_opacity：水印透明度，默认0.5，最大值为1
~~~

## 更新日志 2024-11-11
### Utils

1. LmcqInputValidator：用于验证输入值的类型，可以判断输入是纯数字还是字符串

~~~
功能详解

input_text：    需要验证的输入文本
check_type：    验证类型，可选项：
               - is_digit：判断是否为纯数字
               - is_string：判断是否为字符串（任何非纯数字的输入都被视为字符串）
~~~


## 更新日志 2024-12-12 （模型加密！！！）

### 模型加解密
![img_2.png](img_2.png)
### LmcqModelEncryption
~~~
功能讲解
model_name：选择你要加密的模型
key       ：加密密钥（自定义，用于后续解密的关键密码）
save_name ：加密后的模型名称
~~~
填写完毕之后点击执行，会在你的模型文件夹下生成两个文件，一个模型和一个后缀名为.meta的文件，meta文件记录你的加密签名及版本信息，记住一定要将两个文件放在同一个目录，否则加密后的模型无法解密

### LmcqModelDecryption
~~~
功能讲解
model_name：选择你要解密的模型
key       ：加密密钥（输入加密时设置的密钥信息）
save_name ：解密后的模型名称
~~~

## 更新日志 2024-12-18 （工作流保护！）

### 工作流加解密
![workflow_encryption.png](workflow_encryption.png)

### LmcqWorkflowEncryption
~~~
功能详解：
action:        选择加密或解密操作
password:      加解密密码
workflow_file: 选择要处理的工作流文件（可从根目录或插件的 workflows 文件夹中选择）
save_name:     保存文件的名称
~~~

该节点允许你对工作流文件进行加密保护，防止未经授权的访问。加密后的工作流文件需要使用正确的密码解密后才能使用。

使用方法：
1. 将工作流文件保存到以下任一位置：
   - ComfyUI根目录的 workflows 文件夹（显示为 "root/文件名"）
   - 插件目录的 workflows 文件夹（显示为 "plugin/文件名"）
2. 使用 encrypt 操作创建加密版本
3. 分享加密后的工作流文件
4. 接收者需要使用 decrypt 操作并输入正确的密码才能使用该工作流

注意：加密/解密后的文件会保存在源文件所在的同一文件夹中。

## （LoRA加密！）

### LoRA模型加解密
![lora_encryption.png](lora_encryption.png)

### LmcqLoraEncryption
~~~
功能讲解：
lora_name：选择要加密的LoRA模型
key：      加密密钥（自定义，用于后续解密的关键密码）
save_name：加密后的模型名称
~~~
执行后会在 loras/encrypted 文件夹下生成两个文件：加密后的模型和一个后缀为 .meta 的文件。meta 文件记录了加密签名和版本信息，这两个文件必须放在同一目录才能成功解密。

### LmcqLoraDecryption
~~~
功能讲解：
lora_name：选择要解密的LoRA模型
key：      加密密钥（输入加密时设置的密钥信息）
save_name：解密后的模型名称
~~~
解密后的LoRA模型会保存在 loras/decrypted 文件夹中。



## 参与贡献!zebord
## SakuraLLMServer
- 运行 [SakuraLLM](https://github.com/SakuraLLM/SakuraLLM) 轻小说翻译模型的服务器端一键包
- 可作为 [AiNiee](https://github.com/NEKOparapa/AiNiee)、[GalTransl](https://github.com/xd2333/GalTransl)、[轻小说翻译机器人](https://books.fishhawk.top/workspace/sakura) 等翻译器的服务器端使用
- 结合本页中的设置指南，可以得到最优化的性能，相较于默认设置可提升 3-5 倍

## 要求
- 至少 8G 显存的 Nvidia 独立显卡
- 确保安装了 `最新版本` 的显卡驱动程序

## 步骤
- 从 [发布页](https://github.com/neavo/SakuraLLMServer/releases) 下载最新版本的 `SakuraLLMServer` 并解压缩
- 根据显存大小下载适合的模型并放入 `SakuraLLMServer` 文件夹。

| 显存大小         | 模型规模     | 下载链接                                                  |
|:---------------:|:-----------:|:---------------------------------------------------------:|
| 8G/10G          | 7B          | [GalTransl-7B-v2.6-IQ4_XS.gguf](https://huggingface.co/SakuraLLM/GalTransl-7B-v2.6/blob/main/GalTransl-7B-v2.6-IQ4_XS.gguf)|
| 11G/12G/16G/24G | 14B         | [Sakura-14B-Qwen2.5-v1.0pre1-GGUF](https://huggingface.co/SakuraLLM/Sakura-14B-Qwen2.5-v1.0pre1-GGUF/blob/main/sakura-14b-qwen2.5-v1.0-iq4xs.gguf) |

## 启动
- 现在你的文件结构应该类似于：
```
  SakuraLLMServer\llama\...
                    \00_Core.bat
                    \01_1280_NP16.bat
                    \sakura-14b-qwen2.5-v1.0-iq4xs.gguf
                    \...
```
- 根据 `你的显存和模型的搭配组合` 选择对应的启动脚本，双击启动即可
  
| 显存大小         | 模型规模     | 启动脚本             |
|:---------------:|:-----------:|:--------------------:|
| 8G              | 7B          | 01_1280_NP4_KVQ8.bat |
| 10G             | 7B          | 01_1280_NP8_KVQ8.bat |
| 11G             | 14B         | 01_1280_NP4.bat |
| 12G             | 14B         | 01_1280_NP6.bat |
| 16G/24G         | 14B         | 01_1280_NP16.bat |

## 设置 AiNiee 
- 本节为搭配 [AiNiee](https://github.com/NEKOparapa/AiNiee) 翻译器使用时的设置教程，使用其他翻译器时可以跳过
- 确保安装了 `最新测试版本（版本号 >= 4.75）` 的 [AiNiee](https://github.com/NEKOparapa/AiNiee) 应用
- 启动应用，并根据 `显存大小` 设置以下选项：
  
| 选项 | 设置 |
|------|:----:|
| 账号设置 - SakuraLLM - 请求地址 | http://127.0.0.1:8080 |
| 账号设置 - SakuraLLM - 模型选择（8G/10G） | Sakura-v0.9 |
| 账号设置 - SakuraLLM - 模型选择（11G/12G/16G/24G） | Sakura-v1.0 |
| 翻译设置 - 发送设置 - 使用 Tokens 模式 | 启用 |
| 翻译设置 - 发送设置 - 每次翻译 Tokens | 512 |
| 翻译设置 - 发送设置 - 最大线程数 | 启动脚本名称中 NP 后的数字 |
| 翻译设置 - 发送设置 - 错误重翻最大次数限制 | 1 |
| 翻译设置 - 发送设置 - 翻译流程最大轮次限制 | 99 |
| 翻译设置 - 专项设置 - 执行替换后翻译 | 启用 |
| 翻译设置 - 专项设置 - 处理首尾非字符文本 | 启用 |

## 设置 GalTransl（TODO） 
## 设置 轻小说翻译机器人（TODO） 

## 常见问题
- 什么是 `爆显存`，会导致什么问题？
  - 系统需求的显存超过了显卡实际的物理显存大小，称之为 `爆显存`
  - `爆显存` 时，翻译的速度和结果都会出现异常，基本丧失可用性，所以要避免这种情况
    
- 如何判断是否 `爆显存`
  - 如果爆的比较厉害，程序会直接报错或者退出
  - 爆了一点又没有完全爆比较难判断
  - 一个可参考的方式是通过第三方软件监测显卡功耗
  - 满载执行任务时，显卡实际功耗应为最大功耗的 `70%-80%` 或者更高
  - 如果显存接近用完，但是显卡实际功耗很低，则大概率是爆显存了

- 如何避免 `爆显存`
  - 在模型启动后，模型占用的显存大小是固定的，不会变化
  - 但是系统中的其他应用，比如 `浏览器`、`动态壁纸`、`视频播放器` 及所有基于浏览器内核的应用（`QQNT`、`VSCODE` 等）都会占用显存
  - 本项目中的脚本都预留了一定的冗余空间，但如果开启过多应用，依然可能导致显存消耗完
  - 所以在使用时，应尽量减少开启其他消耗显存的应用

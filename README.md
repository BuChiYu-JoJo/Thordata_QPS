# Thordata_QPS / Brightdata_QPS

基于时间窗口的 SERP 性能压测脚本，支持多引擎、多并发阶梯、CSV 统计输出。  
请求参数构造保持不变（适配 Brightdata SERP 所需的 engine/q 等参数），其余并发、统计、CSV 逻辑与 Thordata 主分支保持一致。

## 快速开始

```bash
# Thordata 默认
python Thordata_QPS.py -k <API_KEY> --all-engines -t 60 -c 5

# Brightdata（保持请求参数不变，仅切换主机/产品标识）
python Thordata_QPS.py -k <API_KEY> --product Brightdata --host serp.brightdata.com \
  --request-path /request --all-engines -t 60 -c 5

# 并发阶梯测试
python Thordata_QPS.py -k <API_KEY> -e google bing -t 30 --concurrency-steps 5 10 20

# 保存详细请求记录
python Thordata_QPS.py -k <API_KEY> -e google -t 30 -c 5 --save-details
```

## 主要特性
- **时间驱动的并发测试**：按设定秒数持续发送请求，避免固定次数造成尾部超时。
- **并发阶梯**：`--concurrency-steps` 支持多轮并发配置连续运行。
- **CSV 输出**：自动生成汇总统计表，开启 `--save-details` 另存单请求明细。文件前缀按 `--product` 区分。
- **多引擎支持**：保持 Brightdata/Thordata 兼容的请求参数构造。

## 常用参数
- `--host`：API 主机（Brightdata 例：`serp.brightdata.com`）。  
- `--request-path`：API 路径（默认 `/request`）。  
- `--product`：产品标签，用于 CSV/统计前缀。  
- `--all-engines` 或 `-e`：选择测试的引擎。  
- `-t/--duration`：单轮运行时长（秒）。  
- `-c/--concurrency`：并发数；或使用 `--concurrency-steps` 提供多档并发。  
- `--save-details`：输出单请求明细 CSV。  
- `-o/--output`：自定义汇总文件名，缺省时按产品自动命名。

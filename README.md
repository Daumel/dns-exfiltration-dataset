# DNS exfiltration dataset

The dataset, which you can download from [Kaggle](https://www.kaggle.com/datasets/daumel/dns-tunneling-dataset), is
provided in CSV format and includes both benign DNS traffic and malicious DNS exfiltration traffic. Each
row
represents a query-response pair and consists of 45 attributes. Below is an example illustrating the
structure of the dataset:

| flow_id                                                              | timestamp                  | src_ip        | src_port | dst_ip       | dst_port | duration | total_bytes | receiving_bytes | sending_bytes | packets_rate | packets_len_rate | min_packets_len | max_packets_len | mean_packets_len | standard_deviation_packets_len | variance_packets_len | coefficient_of_variation_packets_len | dns_domain_name                                   | dns_top_level_domain | dns_second_level_domain | dns_domain_name_length | dns_subdomain_name_length | uni_gram_domain_name                              | bi_gram_domain_name                               | tri_gram_domain_name                              | numerical_percentage | character_distribution                            | character_entropy | max_continuous_numeric_len | max_continuous_alphabet_len | max_continuous_consonants_len | max_continuous_same_alphabet_len | vowels_consonant_ratio | conv_freq_vowels_consonants | distinct_ttl_values | ttl_values_min | ttl_values_max | ttl_values_mean | ttl_values_mode | ttl_values_median | distinct_A_records | ans_resource_record_type | ans_resource_record_class | label     |
|----------------------------------------------------------------------|----------------------------|---------------|----------|--------------|----------|----------|-------------|-----------------|---------------|--------------|------------------|-----------------|-----------------|------------------|--------------------------------|----------------------|--------------------------------------|---------------------------------------------------|----------------------|-------------------------|------------------------|---------------------------|---------------------------------------------------|---------------------------------------------------|---------------------------------------------------|----------------------|---------------------------------------------------|-------------------|----------------------------|-----------------------------|-------------------------------|----------------------------------|------------------------|-----------------------------|---------------------|----------------|----------------|-----------------|-----------------|-------------------|--------------------|--------------------------|---------------------------|-----------|
| 2023-08-05 09:03:40.862988_192.168.68.62_49517_192.168.68.1_53_11349 | 2023-08-05 09:03:40.862988 | 192.168.68.62 | 49517    | 192.168.68.1 | 53       | 0.38218  | 178         | 105             | 73            | 5.23314      | 465.74915        | 73              | 105             | 89.0             | 16.0                           | 256.0                | 0.17978                              | athomenet.com.                                    | com                  | athomenet.com           | 14                     | 0                         | ['a', 't', 'h', 'o', 'm', 'e', 'n', 'e', 't', ... | ['at', 'th', 'ho', 'om', 'me', 'en', 'ne', 'et... | ['ath', 'tho', 'hom', 'ome', 'men', 'ene', 'ne... | 0.0                  | {'o': 2, 'm': 2, 'c': 1, 'a': 1, 'n': 1, 't': ... | 3.093069          | 0                          | 9                           | 2                             | 1                                | 0.714286               | 0.642857                    | 1                   | 300            | 300            | 300.0           | 300.0           | 300.0             | 2                  | [1, 1]                   | [1, 1]                    | Benign    |
| 2019-04-29 07:57:43.810610_198.41.0.130_36304_198.41.0.10_53_18725   | 2019-04-29 07:57:43.810610 | 198.41.0.130  | 36304    | 198.41.0.10  | 53       | 0.00547  | 665         | 373             | 292           | 365.89933    | 121661.52665     | 292             | 373             | 332.5            | 40.5                           | 1640.25              | 0.1218                               | bba9012dfe6eb0362db5ae43cc103986d2d96b5c1d937c... | com                  | dnscat-txt.com          | 233                    | 60                        | ['b', 'b', 'a', '9', '0', '1', '2', 'd', 'f', ... | ['bb', 'ba', 'a9', '90', '01', '12', '2d', 'df... | ['bba', 'ba9', 'a90', '901', '012', '12d', '2d... | 0.549356             | {'9': 11, '6': 19, 'd': 14, 'm': 1, 'n': 1, '-... | 4.201555          | 7                          | 6                           | 4                             | 2                                | 0.462687               | 0.077253                    | 1                   | 60             | 60             | 60.0            | 60.0            | 60.0              | 0                  | [16]                     | [1]                       | Malicious |

The dataset comprises 1,106,303 benign samples and 723,178 malicious samples. The malicious samples were generated using
nine different DNS exfiltration tools, including iodine, dnsexfiltrator, cobaltstrike, and others.

## Assumptions regarding DNS exfiltration

Malware using DNS exfiltration typically attempts to mimic legitimate traffic to avoid detection. With this context, two
assumptions were made when generating the dataset:

1. Like benign DNS queries, DNS exfiltration queries use randomly selected Transaction IDs. Consequently, traffic
   generated by DNS-Shell was excluded from this dataset, as this tool reuses
   the same Transaction ID across consecutive queries.
2. DNS exfiltration domains follow the LDH rule as defined
   in [RFC 1035](https://www.ietf.org/rfc/rfc1035.txt), meaning domain names are limited to letters, digits, and
   hyphens. Therefore, traffic from Iodine that uses Base128 encoding was also
   excluded, as it does not conform to the LDH rule.

## Why was the dataset generated?

There is a lack of publicly available DNS exfiltration datasets that are both high-quality and provided in an analyzable
format. Most existing CSV datasets exhibit one or more of the following issues:

- The DNS exfiltration traffic was generated using only one or a few exfiltration tools, resulting in malicious samples
  that
  do not represent a diverse range of attack patterns.
- There are limited attributes per sample, which restricts the extensive analysis of the behaviors of DNS exfiltration
  tools.
- The datasets contain inaccurate data, including erroneous labeling (e.g., benign samples labeled as malicious).
- There is insufficient explanation regarding the dataset's creation process, including details about the exfiltration
  tools that were utilized.

This dataset aims to address these challenges.

## How was the dataset generated?

This dataset is built upon PCAP files obtained from the following sources:

- **[PCAP files by Singh et al.](https://data.mendeley.com/datasets/zh3wnddzxy/1), used in their
  work, [Detecting bot-infected machines using DNS fingerprinting](https://www.sciencedirect.com/science/article/abs/pii/S174228761830272X?via%3Dihub):**  
  Benign university traffic captured on a Monday between 7 a.m. and 7 p.m.

- **[PCAP files by Gao et al.](https://github.com/ggyggy666/DNS-Tunnel-Datasets), used in their
  work, [GraphTunnel: Robust DNS Tunnel Detection Based on DNS Recursive Resolution Graph](https://ieeexplore.ieee.org/document/10636232):**  
  Top 1 million domains' benign traffic and malicious traffic generated by the
  tools [tcp-over-dns](https://github.com/sunapi386/tcp-over-dns) and [dnspot](https://github.com/mosajjal/dnspot).

- **[PCAP files by Chen et al.](https://github.com/chenshaojie-happy/DNS-covert-channel-detection-method-using-the-LSTM-model/tree/main),
  used in their
  work, [DNS Covert Channel Detection Method Using the LSTM Model](https://www.sciencedirect.com/science/article/abs/pii/S0167404820303680):**  
  Malicious traffic generated by the
  tools [cobaltstrike](https://www.cobaltstrike.com), [dns2tcp](https://github.com/alex-sector/dns2tcp), [dnscat2](https://github.com/iagox86/dnscat2), [iodine](https://github.com/yarrick/iodine),
  and [ozymandns](https://github.com/splitbrain/dnstunnel).

- **My own network environment:**  
  Malicious traffic generated by the tools [dnsexfiltrator](https://github.com/Arno0x/DNSExfiltrator) and
  a [modified version](https://github.com/kristijanziza/dns) of dnsexfiltrator.

The original PCAP files were
subsequently converted to CSV format using the
tool [ALFlowLyzer](https://github.com/ahlashkari/ALFlowLyzer/tree/main).
ALFlowLyzer is a Python open-source project to extract application layer features from network traffic. It was developed
by Shafi et al. to generate
the [BCCC-CIC-Bell-DNS-2024](https://www.yorku.ca/research/bccc/ucs-technical/cybersecurity-datasets-cds/)
dataset. Further details can be found in their work titled [Unveiling malicious DNS behavior profiling and generating
benchmark dataset through application layer traffic analysis](https://www.sciencedirect.com/science/article/pii/S0045790624003641)

Each row in the CSV files generated by ALFlowLyzer represents a bidirectional DNS flow, with the first packet
determining the forward (source-to-destination) and backward (destination-to-source) directions. The primary identifier
for a DNS flow is the transaction ID in the DNS header. Consequently, if the same transaction ID is reused within a
short timeframe, a single row may correspond to multiple query-response pairs.

This scenario presents a challenge, as multiple query-response pairs with the same transaction ID within a short
timeframe typically occur by chance and are not logically related. This behavior also applies to DNS exfiltration
malware,
which, as
previously noted, typically mimics legitimate traffic by generating separate queries with randomly chosen transaction
IDs.
To address this, all rows containing DNS flows with more than one request and response were removed to ensure each row
accurately represents a unique query-response interaction.

To further enhance the quality of the dataset, additional data cleaning procedures were performed. These
include:

- Removing duplicate query-response pairs.
- Removing incorrectly labeled query-response pairs.
- Removing noisy heartbeat packets that have no exfiltration data in requests and no commands in responses.
- Removing query-response pairs where data was incorrectly extracted due to parsing errors by ALFlowLyzer (which appears
  to have limitations with SRV and MX record types).
- Removing query-response pairs that exhibited erroneous or incomplete responses due to network or nameserver issues.

# What is the meaning of each attribute?

| **No.** | **Attribute**                            | **Description**                                                                                                                                                                                                                                                                                                                  |
|---------|------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 1       | **flow_id**                              | The identifier of the DNS flow, structured as follows: Timestamp_SrcIP_SrcPort_DstIP_DstPort_TransactionID.                                                                                                                                                                                                                      |
| 2       | **timestamp**                            | When the DNS request was sent.                                                                                                                                                                                                                                                                                                   |
| 3       | **src_ip**                               | The IP address from which the DNS request was sent.                                                                                                                                                                                                                                                                              |
| 4       | **src_port**                             | The port from which the DNS request was sent.                                                                                                                                                                                                                                                                                    |
| 5       | **dst_ip**                               | The IP address to which the DNS request was sent.                                                                                                                                                                                                                                                                                |
| 6       | **dst_port**                             | The port to which the DNS request was sent.                                                                                                                                                                                                                                                                                      |
| 7       | **duration**                             | The number of seconds between the DNS request and the DNS response.                                                                                                                                                                                                                                                              |
| 8       | **total_bytes**                          | The total size of the DNS request and its corresponding DNS response, measured in bytes.                                                                                                                                                                                                                                         |
| 9       | **receiving_bytes**                      | The size of the DNS response in bytes.                                                                                                                                                                                                                                                                                           |
| 10      | **sending_bytes**                        | The size of the DNS request in bytes.                                                                                                                                                                                                                                                                                            |
| 11      | **packets_rate**                         | The rate at which DNS packets are sent or received during the DNS flow. A higher packets rate means that the DNS request and response were exchanged in a shorter timeframe, suggesting fast communication. This metric is calculated by dividing the number of packets by the duration of the DNS flow: 2 / duration.           |
| 12      | **packets_len_rate**                     | The rate at which the total size of DNS packets is sent or received during the DNS flow. A higher packets_len_rate means that a larger amount of data was transmitted relative to the duration. This metric is calculated by dividing the total size of the DNS packets by the duration of the DNS flow: total_bytes / duration. |
| 13      | **min_packets_len**                      | The size in bytes of the smallest packet within the DNS flow.                                                                                                                                                                                                                                                                    |
| 14      | **max_packets_len**                      | The size in bytes of the largest packet within the DNS flow.                                                                                                                                                                                                                                                                     |
| 15      | **mean_packets_len**                     | The average size in bytes of the packets within the DNS flow.                                                                                                                                                                                                                                                                    |
| 16      | **standard_deviation_packets_len**       | The standard deviation of packet sizes in the DNS flow.                                                                                                                                                                                                                                                                          |
| 17      | **variance_packets_len**                 | The variance of packet sizes in the DNS flow.                                                                                                                                                                                                                                                                                    |
| 18      | **coefficient_of_variation_packets_len** | The coefficient of variation of packet sizes in the DNS flow. This metric is calculated by dividing the standard deviation of packet sizes by the average packet size: standard_deviation_packets_len / mean_packets_len.                                                                                                        |
| 19      | **dns_domain_name**                      | The domain name for which information is being queried.                                                                                                                                                                                                                                                                          |
| 20      | **dns_top_level_domain**                 | The top-level domain of the queried domain name.                                                                                                                                                                                                                                                                                 |
| 21      | **dns_second_level_domain**              | The second-level domain (including the first-level domain) of the queried domain name.                                                                                                                                                                                                                                           |
| 22      | **dns_domain_name_length**               | The length of the domain name (including the period at the end, which signifies the root level), measured in characters.                                                                                                                                                                                                         |
| 23      | **dns_subdomain_name_length**            | The length of the first subdomain when read from left to right.                                                                                                                                                                                                                                                                  |
| 24      | **uni_gram_domain_name**                 | The unigram of the domain name.                                                                                                                                                                                                                                                                                                  |
| 25      | **bi_gram_domain_name**                  | The bigram of the domain name.                                                                                                                                                                                                                                                                                                   |
| 26      | **tri_gram_domain_name**                 | The trigram of the domain name.                                                                                                                                                                                                                                                                                                  |
| 27      | **numerical_percentage**                 | The percentage of numeric characters in the domain name relative to its total length. This metric is calculated by dividing the number of digits in the domain name by its length: number of digits in the domain name / dns_domain_name_length.                                                                                 |
| 28      | **character_distribution**               | The frequency distribution of individual characters in the domain name. This attribute outputs the number of occurrences of each character in the domain name as a dictionary, where the keys are the characters and the values are their frequencies.                                                                           |
| 29      | **character_entropy**                    | The entropy of the characters in the domain name.                                                                                                                                                                                                                                                                                |
| 30      | **max_continuous_numeric_len**           | The length of the longest continuous sequence of digits in the domain name.                                                                                                                                                                                                                                                      |
| 31      | **max_continuous_alphabet_len**          | The length of the longest continuous sequence of alphabetic characters in the domain name.                                                                                                                                                                                                                                       |
| 32      | **max_continuous_consonants_len**        | The length of the longest continuous sequence of consonants in the domain name.                                                                                                                                                                                                                                                  |
| 33      | **max_continuous_same_alphabet_len**     | The length of the longest continuous sequence of identical letters in the domain name.                                                                                                                                                                                                                                           |
| 34      | **vowels_consonant_ratio**               | The ratio of vowels to consonants in the domain name.                                                                                                                                                                                                                                                                            |
| 35      | **conv_freq_vowels_consonants**          | The relative frequency of alternating vowels and consonants in the domain name. This metric is calculated by dividing the number of occurrences of alternating letters (vowel followed by consonant or vice versa) by the total length of the domain name.                                                                       |
| 36      | **distinct_ttl_values**                  | The number of distinct TTL values present in the resource records of the DNS flow.                                                                                                                                                                                                                                               |
| 37      | **ttl_values_min**                       | The smallest TTL value among the resource records in the DNS flow.                                                                                                                                                                                                                                                               |
| 38      | **ttl_values_max**                       | The largest TTL value among the resource records in the DNS flow.                                                                                                                                                                                                                                                                |
| 39      | **ttl_values_mean**                      | The average TTL value among the resource records in the DNS flow.                                                                                                                                                                                                                                                                |
| 40      | **ttl_values_mode**                      | The TTL value that occurs most frequently among the resource records in the DNS flow.                                                                                                                                                                                                                                            |
| 41      | **ttl_values_median**                    | The median of the TTL values among the resource records in the DNS flow.                                                                                                                                                                                                                                                         |
| 42      | **distinct_A_records**                   | The number of distinct resource records of type A present in the DNS flow.                                                                                                                                                                                                                                                       |
| 43      | **ans_resource_record_type**             | The types of resource records in the answer section.                                                                                                                                                                                                                                                                             |
| 44      | **ans_resource_record_class**            | The classes of resource records in the answer section.                                                                                                                                                                                                                                                                           |
| 45      | **label**                                | Indicates whether the DNS flow is classified as "Malicious" or "Benign."                                                                                                                                                                                                                                                         |

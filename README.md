# YARA Rule: Malrev Ransomware Detection

A YARA rule for identifying suspicious Linux ELF files that may be associated with an obfuscated Malrev ransomware variant.

## 📌 About the Project

This repository contains `malrev_detect.yar`, a YARA rule created to detect selected indicators associated with a suspected obfuscated Malrev ransomware sample.

The rule looks for a combination of:
- A Linux ELF file header.
- The cryptographic-related string `EVP_aes_128_gcm`.
- One or more obfuscated virtualization/container indicators or fragments of suspicious system paths.

The rule is intended for educational use and defensive malware analysis. A match indicates that the file meets the rule's conditions; it does not, by itself, prove that the file is malicious.

## 🔍 Detection Logic

The rule is named `Linux_Ransomware_Malrev_Obfuscated` and checks for the following indicators:

| Indicator | Purpose |
|---|---|
| ELF header | Restricts matching to Linux ELF-format files. |
| `EVP_aes_128_gcm` | Looks for a cryptography-related string. |
| `VirtualBH` | Obfuscated or altered indicator resembling `VirtualBox`. |
| `VMwa` | Partial string resembling `VMware`. |
| `/.dockerH` | Altered fragment resembling `/.dockerenv`. |
| `/sys/claH` and `id/produH` | Fragments of a potentially altered system path. |

The condition requires the ELF header and cryptography-related string, plus at least one of the configured obfuscated indicators or path fragments.

These strings are specific to the current rule and should be validated against known samples. They are not a comprehensive signature for all Malrev variants.

## 🧰 Requirements

- [YARA](https://github.com/VirusTotal/yara)
- A Linux, macOS, or Windows environment capable of running YARA

Check whether YARA is installed:

```bash
yara --version
```

## 🚀 Getting Started

### 1. Clone the Repository

```bash
git clone https://github.com/LaOdhe16/YaraRule_MalRev_Detection.git
cd YaraRule_MalRev_Detection
```

### 2. Scan a File

Run the rule against a file you want to inspect:

```bash
yara malrev_detect.yar /path/to/suspicious_file
```

Replace `/path/to/suspicious_file` with the actual file path.

### 3. Scan a Directory

To scan files recursively on Linux, use:

```bash
find /path/to/samples -type f -print0 | xargs -0 -I {} yara malrev_detect.yar "{}"
```

Replace `/path/to/samples` with the directory containing files you are authorized to analyze.

## 🧪 Interpreting Results

- **A rule name and file path are printed:** the file matched the rule's conditions and should be investigated further.
- **No output is printed:** none of the scanned files matched this rule.
- **A match is not a final verdict:** legitimate files may contain similar strings, and malicious samples may evade this rule.
- **No match does not mean a file is safe:** this rule only checks a limited set of indicators.

For stronger confidence, combine YARA results with static analysis, sandbox analysis in an isolated environment, file hashes, and other threat-intelligence sources.

## 🛡️ Safe Usage

- Use this rule for defensive analysis, education, and authorized security testing.
- Do not execute suspicious samples on your everyday machine.
- Analyze potentially malicious files in an isolated environment.
- Validate the rule against known benign and malicious samples before using it in production.

## 🎯 Project Objectives

- Practice writing and understanding YARA rules.
- Explore static indicators used in Linux malware analysis.
- Learn how string matching and logical conditions can be combined for detection.
- Support defensive malware research and threat hunting.


## 📚 References

- [YARA official repository](https://github.com/VirusTotal/yara)
- [YARA documentation](https://yara.readthedocs.io/)

## 📄 License

No license file is currently specified in this repository. Unless a license is added, reuse and redistribution may be subject to copyright restrictions.

---

**Disclaimer:** This rule is a detection aid for research and defensive security purposes. It may produce false positives and false negatives and should not be treated as a standalone malware verdict.

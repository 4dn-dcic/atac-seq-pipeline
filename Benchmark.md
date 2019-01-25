## Benchmarking for ATAC-seq.

The following is a tentative benchmarking result based on a test sample.
* **Measured** : the measured values for the test sample(s).
* **Recommended/Estimated** : tentative values of choice for running the future samples.

### Whole pipeline, with a single biological replicate.

|   | **Measured** | **Recommended/Estimated** | **Measured after Recommended run** | **Recommended/Estimated (adjusted)** |
| - | -------- | --------- | -------- | --------- |
| input size | 1.2 * 2 + 1.5 * 2 = 5.2GB (fastq) + 3.9GB (bowtie) | - | 1.2 * 2 + 1.5 * 2 = 5.2GB (fastq) + 3.9GB (bowtie) | - |
| param | `atac.bowtie2.cpu`= 16 | `atac.bowtie2.cpu`= 4 | `atac.bowtie2.cpu`= 4 | `atac.bowtie2.cpu`= 4 |
| mem | 4.5G | 8GB | 6.9GB | 8GB |
| disk | 50% (48.4GB) | 10x fastq + bowtie (x1.5 to account for sample variability) | 77.3% (71.4GB) | 10x fastq + bowtie (x1.5 to account for sample variability) |
| CPU | 99.9% (16cores) | `atac.bowtie2.cpu` | 99.4% most of the time (very good) | `atac.bowtie2.cpu` |
| runtime | 7hr | 1.3h x fastq for 16-core c instance | 11.5hr | 2.2h x fastq for 4-core c5 instance |
| instance | c4.4xlarge | c5.xlarge (8GB mem, 4 cores, $0.17/hr) | c5.xlarge | c5.xlarge |
| estiamted runtime on recommended instance | - | 4.5h x fastq (24hr for the current input) | - | 2.2h x fastq |
| output size | 827MB (tas) | 0.2x fastq | 827MB (tas) | 0.2x fastq |
| AWSEM job id | UNA6eu2l9L5v | - | 8kL7te9yMcEd | - |
| cost | $5.45 | ~$1 per GB fastq | $1.92 | ~$0.37 per GB fastq |

* notes : `atac.bowtie2.cpu`= 4 is recommended because memory is fixed to 8GB and the maximum number of CPUs on AWS instances with 8GB mem is 4.


### Aln (align-only=true, starting from fastq) with a single biological replicate

|   | **Recommended/Estimated (from whole pipeline estimate)** | **Measured after Recommended run** | **Recommended/Estimated (adjusted)** |
| - | --------- | -------- | --------- |
| input size | - | 1.2 * 2 + 1.5 * 2 = 5.2GB (fastq) + 3.9GB (bowtie) | - |
| param | `atac.bowtie2.cpu`= 4 | `atac.bowtie2.cpu`= 4 | `atac.bowtie2.cpu`= 4 |
| mem | 8GB | 6.9GB | 8GB |
| disk | 10x fastq + bowtie (x1.5 to account for sample variability) | 77.3% (71.4GB) | 10x fastq + bowtie (x1.5 to account for sample variability) |
| CPU | `atac.bowtie2.cpu` | 99.4% most of the time (very good) | `atac.bowtie2.cpu` |
| runtime | 2.2h x fastq for 4-core c5 instance | 10hr | 1.9h x fastq for 4-core c5 instance |
| instance | c5.xlarge (8GB mem, 4 cores, $0.17/hr) | c5.xlarge | c5.xlarge |
| estiamted runtime on recommended instance | 2.2h x fastq | - | 1.9h x fastq |
| output size | 0.2x fastq | 827MB (tas) | 0.2x fastq |
| AWSEM job id | - | DFudNc9O6t60 | - |
| cost | ~$0.37 per GB fastq | $1.65 | ~$0.32 per GB fastq |

* notes : `atac.bowtie2.cpu`= 4 is recommended because memory is fixed to 8GB and the maximum number of CPUs on AWS instances with 8GB mem is 4.


### Postaln (align-only=false, starting from tas) with a single biological replicate


|   | **Measured** | **Recommended/Estimated** | **Measured after Recommended run** | **Recommended/Estimated (adjusted)** |
| - | -------- | --------- | -------- | --------- |
| input size | 827MB (tas) | - | 827MB (tas) | - |
| param | - | - | - | - |
| mem | 7.3G | 8GB | 7.1GB | 8GB |
| disk | 44.7% (43.9GB) | 55x tas (x1.5 to account for sample variability) | 66.7% (43.9GB) | 55x tas (x1.5 to account for sample variability) |
| CPU | 67.7% (11) | 12 | 64% (11) | 12 |
| runtime | 2hr | 2.4h x tas for c instance | 1hr 20min | 1.6h x tas for c5 instance |
| instance | c4.4xlarge | c5.4xlarge (32GB mem, 16 cores, $0.68/hr) | c5.4xlarge | c5.4xlarge |
| estimated runtime on recommended instance | - | 2.4h x tas (2hr for the current input) | - | 1.6h x tas |
| output size | 1.3G (sig_fc) + 9.3MB x 2 (bb) = 1.3G | 1.6x tas | 1.3G (sig_fc) + 9.3MB x 2 (bb) = 1.3G | 1.6x tas |
| AWSEM job id | R6NetOYXEmq8 | - | JayG50nKBqXT | - |
| cost | $1.67 | - | $0.9 | ~$1 per GB tas |


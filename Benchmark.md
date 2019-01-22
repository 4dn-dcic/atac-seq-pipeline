## Benchmarking for ATAC-seq.

The following is a tentative benchmarking result based on a test sample.
* **Measured** : the measured values for the test sample(s).
* **Recommended/Estimated** : tentative values of choice for running the future samples.

### Aln (align-only=true, starting from fastq)
* input size : 1.2 * 2 + 1.5 * 2 = 5.2GB (fastq) + 3.9GB (bowtie)
* param : `atac.bowtie2.cpu`= 16

|   | **Measured** | **Recommended/Estimated** |
| - |-------- | --------- | 
| mem | 4.5G | 8GB |
| disk | 50% (48.4GB) | 10x fastq + bowtie |
| CPU | 99.9% (16cores) | `atac.bowtie2.cpu` |
| runtime | 7hr | 1.3h x fastq for 16-core c instance |
| instance | c4.4xlarge | c5.xlarge (8GB mem, 4 cores, $0.17/hr) |
| runtime on recommended instance | - | 6.5h x fastq (35hr for the current input) |

### Postaln (align-only=false, starting from tas)

* input size : 827MB (tas) - output of the above Aln run.
* param : -

|   | **Measured** | **Recommended/Estimated** |
| - |-------- | --------- |
| mem | 4.2G | 8GB |
| disk | 19.4% (19GB) | 25x tas |
| CPU | 18% | 1 |
| runtime | 2hr | 2.4h x tas for c instance |
| instance | c4.4xlarge | m5a.large (8GB mem, 2 cores, $0.086/hr) |
| runtime on recommended instance | - | 3h x fastq (2.5hr for the current input) | 



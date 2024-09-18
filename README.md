# ACGT - Czech Reference Healthy Genomes of 1290 Individuals

This repository contains a step-by-step tutorial to annotate your genomic variants using ACGT data.

---

## 1. Download the Reference File

First, download the final VCF file along with its index from the following link:

[ACGT Reference File](https://owncloud.cesnet.cz/index.php/s/jPI2wdq1P7AF9uJ)

---

## 2. Normalize Your VCF

To ensure compatibility ([see](https://gatk.broadinstitute.org/hc/en-us/community/posts/15706942393371-Error-when-running-VariantAnnotator?page=1#community_comment_29137996930971)) with **GATK VariantAnnotator**, you need to normalize your `.vcf` file using `bcftools`. This will split and re-align variants. Run the following command:

```bash
bcftools norm -m -both -o your.normalized.vcf.gz -Oz your.input.vcf.gz
```
---

## 3. Annotate Your VCF

Now, run the ACGT annotation with the following command:

```bash
gatk VariantAnnotator \
  -V your.normalized.vcf.gz \
  -O your.annontated.ACGT.vcf.gz \
  --resource:ACGT acgt.ACcounts.noSamples.normalized.vcf.gz \
  --expression ACGT.AF \
  --expression ACGT.AC \
  --expression ACGT.AC_Hom \
  --expression ACGT.AC_Het \
  --expression ACGT.AC_Hemi
```

If a variant in your `.vcf` file is also present in the ACGT reference `.vcf`, the following tags will be appended to the `INFO` field:

- **ACGT.AC**: The allele count in ACGT samples
- **ACGT.AF**: The allele frequency in ACGT samples
- **ACGT.AC_Hom**: The number of alleles observed in homozygous genotypes
- **ACGT.AC_Het**: The number of alleles observed in heterozygous genotypes
- **ACGT.AC_Hemi**: The number of alleles observed in hemizygous genotypes

## More Information

For more thorough information, including geographical metadata, visit our online web application:

🌐 [ACGT Database](https://database.acgt.cz/)

---

## Citation

When using data from this database, please cite our project in the Acknowledgments or Funding section of your publication, in either Czech or English.

**CZ**:  
_CZ: Tato práce vznikla za podpory projektu A-C-G-T, reg. č. CZ.02.1.01/0.0/0.0/16_026/0008448 financovaného z EFRR.._

**EN**:  
_EN: This work was supported from European Regional Development Fund-Project „A-C-G-T“ (No. CZ.02.1.01/0.0/0.0/16_026/0008448)_


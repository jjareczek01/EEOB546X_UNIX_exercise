# EEOB546X_UNIX_exercise
Repo for the UNIX assignment in EEOB546X

## Workflow outline
1. Examine files to see what information they contain: head -n 3 snp_position.txt and head -n 3 fang_et_al_genotypes.txt
⋅⋅Alter files as necessary to contain only what information is needed
2. Separate out maize and teosinte data from the genome file, based on identifiers in column 3, and put in new directories
⋅⋅ Sort into maize_genotypes.txt and teosinte_genotypes.txt
3. Transpose genotype files using awk script: awk -f transpose.awk genotype_files > genotype_files_transposed.txt
4. Join the snp_position.txt and transposed genotype files
5. Split genotype files into chromosome files, one for each chromosome
⋅⋅A Sort chromosome files twice--one in ascending order by SNP position, and one in descending order
⋅⋅B Replace missing values with ? (increasing position) and - (decreasing position)
⋅⋅C Place SNPS with unknown positions in a separate file
⋅⋅D Place SNPS with multiple positions in a separate file
⋅⋅⋅ Might be easier to do C and D before chromosome split; will test
6. Obtain file size information with du and word count information with wc for each file

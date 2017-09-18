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

## Workflow
head -n 3 fang_et_al_genotypes.txt
head -n 3 snp_position.txt

#only columns 1, 3, and 4 in snp_position.txt are necessary for the assignment
cut -f 1,3,4 snp_positions.txt > cut_snps.txt
#cut_snps.txt needs to be sorted; simply using sort will sort the headers in with the data
(head -n 1 cut_snps.txt && tail -n +2 cut_snps.txt | sort -k1,1) > cut_snps_sorted.txt
#snp positions are now ready to proceed

#splitting fang_et_al_genotypes into maize and teosinte
grep 'Sample_ID\|ZMMIL\|ZMMLR\|ZMMMR' fang_et_al_genotypes.txt > maize_genotypes.txt
  #this pulls out the headers as well as any maize samples.
  
#repeat for teosinte
grep 'Sample_ID\|ZMPBA\|ZMPIL\|ZMPJA' fang_et_al_genotypes.txt > teosinte_genotypes.txt

#transpose the genotype files to allow for joining
awk -f transpose.awk maize_genotypes.txt > maize_transposed.txt
awk -f transpose.awk teosinte_genotypes.txt > teosinte_transposed.txt

#Column names differ between the genotype files and the cut_snps file--change to match
sed 's/Sample_ID/SNP_ID/' maize_transposed.txt > maize_correct_headers.txt
sed 's/Sample_ID/SNP_ID/' teosinte_transposed.txt > teosinte_correct_headers.txt

#Genotype files need to be sorted by column 1
(head -n 1 maize_correct_headers.txt && tail -n +2 maize_correct_headers.txt | sort -k1,1) > maize_sorted.txt
(head -n 1 teosinte_correct_headers.txt && tail -n +2 teosinte_correct_headers.txt | sort -k1,1) > teosinte_sorted.txt

#The genotype and SNP position files can now be joined via column 1
join -1 1 -2 1 cut_snps_sorted.txt maize_sorted.txt > maize_joined.txt
join -1 1 -2 1 cut_snps_sorted.txt teosinte_sorted.txt > teosinte_joined.txt

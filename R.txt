#Paper: "DNA barcoding reveals insect diversity in the mangrove ecosystems of the Hainan Island, China" by Lu Liu, Zixiao Guo, Cairong Zhong, Suhua Shi.
#This R script was used to find the Maximum intraspecies genetic distance and Minimum interspecies genetic distance (calculated by MEGA6.0). 
#Please contact with us when you have problems, email:txwana@163.com
#Date: 10-Oct-2018

#YOUR_FILE_DIRECTORY
data <- read.table("Your file", header = F, sep = "\t", stringsAsFactors = F, fill = T)
tmp <- unique(data[, 1])
idx <- tmp[!is.na(tmp)]
result <- data.frame(array(0,c(length(idx),3)))
colnames(result) <- c("spe", "intra_max", "inter_min")
for (i in 1:length(idx)){
        result[i, 1] <- i
        #Maximum intraspecies genetic distance 
        inspe_row <- which(data[, 1] == idx[i])
        inspe_col <- which(data[1, ] == idx[i])
        new_data <- unlist(data[inspe_row, inspe_col])
        result[i, 2] <- max(new_data[!is.na(new_data)])
        #Minimum interspecies genetic distance
        outspe_row <- which(data[, 1] == idx[i])
        outspe_col <- which(data[1, ] != idx[i])
        new_data_1 <- unlist(data[outspe_row, outspe_col])
        outspe_row <- which(data[, 1] != idx[i])
        outspe_col <- which(data[1, ] == idx[i])        
        new_data_2 <- unlist(data[outspe_row, outspe_col])
        new_data <- c(new_data_1, new_data_2)
        result[i, 3] <- min(new_data[!is.na(new_data)])
}

write.table(result, file = "result.txt", col.names = T, row.names = F, quote = F, sep = "\t")
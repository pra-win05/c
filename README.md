#!/bin/bash

# Create root directory
mkdir -p linux-c-programs

# Create subdirectories
mkdir -p linux-c-programs/{1_file_operations,2_directory_operations,3_file_metadata,4_special_files,5_input_output_and_processing,6_misc}

# Create files in each subdirectory
# 1. File Operations
touch linux-c-programs/1_file_operations/{01_create_hello_file.c,02_read_file_contents.c,04_check_file_exists.c,05_rename_file.c,06_delete_file.c,07_copy_file.c,08_move_file_to_dir.c,10_get_file_size.c,15_count_lines.c,16_append_to_file.c,54_delete_delete_me.c,55_rename_oldname_newname.c,56_copy_file_contents.c,57_move_file_to_backup.c,59_size_of_data_txt.c,61_count_lines_log_txt.c,62_append_goodbye_message.c}

# 2. Directory Operations
touch linux-c-programs/2_directory_operations/{03_create_test_dir.c,11_check_test_dir_exists.c,12_create_backup_parent_dir.c,13_list_all_recursive.c,14_delete_all_files_temp_dir.c,25_size_of_directory.c,27_count_files_images_dir.c,35_recursive_delete_temp.c,38_create_date_named_dir.c,40_largest_file_in_dir.c,47_total_dir_size_recursive.c,49_create_timestamp_dir.c,50_create_documents_dir.c,58_list_all_current_dir.c,60_create_pictures_in_parent.c}

# 3. File Metadata
touch linux-c-programs/3_file_metadata/{17_set_readonly_permission.c,18_change_ownership.c,19_get_last_modified_time.c,21_check_file_or_dir.c,32_get_file_type.c,34_get_file_permissions.c,41_check_readable.c,43_check_writable.c,45_get_hardlink_count.c}

# 4. Special Files
touch linux-c-programs/4_special_files/{22_create_hardlink.c,28_create_fifo_myfifo.c,29_read_from_fifo.c}

# 5. I/O Processing
touch linux-c-programs/5_input_output_and_processing/{23_read_csv_data.c,30_truncate_file.c,31_search_string_lines.c,36_read_first_10_lines.c,37_reverse_write_input.c,39_read_binary_file.c,44_execute_shell_commands.c,46_combine_all_text_files.c,48_get_bytes_data_bin.c,52_read_data_txt.c}

# 6. Miscellaneous
touch linux-c-programs/6_misc/{24_get_cwd_absolute_path.c,33_create_empty_file.c,42_move_log_files_to_logs.c,51_check_file_exists.c}

# Create README with navigation
cat > linux-c-programs/README.md << 'EOF'
# Linux System Programming C Programs

## Navigation
[File Operations](#file-operations) | 
[Directory Operations](#directory-operations) | 
[File Metadata](#file-metadata) | 
[Special Files](#special-files) | 
[I/O Processing](#io-processing) | 
[Miscellaneous](#miscellaneous) |
[Answers](#answers)

---

## 1. File Operations <a id="file-operations"></a>
- [`01_create_hello_file.c`](#01-create-hello) - Create "hello.txt"
- [`02_read_file_contents.c`](#02-read-file) - Read file contents
- [`04_check_file_exists.c`](#04-check-file) - Check file existence
- [`05_rename_file.c`](#05-rename-file) - Rename a file
- [`06_delete_file.c`](#06-delete-file) - Delete a file
- [`07_copy_file.c`](#07-copy-file) - Copy file
- [`08_move_file_to_dir.c`](#08-move-file) - Move to directory
- [`10_get_file_size.c`](#10-file-size) - Get file size
- [`15_count_lines.c`](#15-count-lines) - Count lines
- [`16_append_to_file.c`](#16-append-file) - Append to file
- [`54_delete_delete_me.c`](#54-delete-file) - Delete specific file
- [`55_rename_oldname_newname.c`](#55-rename) - Rename files
- [`56_copy_file_contents.c`](#56-copy-contents) - Copy contents
- [`57_move_file_to_backup.c`](#57-move-backup) - Move to backup
- [`59_size_of_data_txt.c`](#59-data-size) - Get data.txt size
- [`61_count_lines_log_txt.c`](#61-count-logs) - Count log lines
- [`62_append_goodbye_message.c`](#62-append-goodbye) - Append message

## 2. Directory Operations <a id="directory-operations"></a>
- [`03_create_test_dir.c`](#03-create-dir) - Create test directory
- [`11_check_test_dir_exists.c`](#11-check-dir) - Check directory exists
- [`12_create_backup_parent_dir.c`](#12-create-nested) - Create nested dirs
- [`13_list_all_recursive.c`](#13-list-recursive) - List recursively
- [`14_delete_all_files_temp_dir.c`](#14-delete-temp) - Delete temp files
- [`25_size_of_directory.c`](#25-dir-size) - Directory size
- [`27_count_files_images_dir.c`](#27-count-images) - Count image files
- [`35_recursive_delete_temp.c`](#35-delete-recursive) - Recursive delete
- [`38_create_date_named_dir.c`](#38-date-dir) - Date-named dir
- [`40_largest_file_in_dir.c`](#40-largest-file) - Find largest file
- [`47_total_dir_size_recursive.c`](#47-recursive-size) - Recursive size
- [`49_create_timestamp_dir.c`](#49-timestamp-dir) - Timestamp dir
- [`50_create_documents_dir.c`](#50-create-docs) - Create docs dir
- [`58_list_all_current_dir.c`](#58-list-current) - List current dir
- [`60_create_pictures_in_parent.c`](#60-pictures-parent) - Create in parent

## 3. File Metadata <a id="file-metadata"></a>
- [`17_set_readonly_permission.c`](#17-readonly) - Set read-only
- [`18_change_ownership.c`](#18-change-owner) - Change ownership
- [`19_get_last_modified_time.c`](#19-mod-time) - Get mod time
- [`21_check_file_or_dir.c`](#21-file-or-dir) - Check file/dir
- [`32_get_file_type.c`](#32-file-type) - Get file type
- [`34_get_file_permissions.c`](#34-permissions) - Get permissions
- [`41_check_readable.c`](#41-check-readable) - Check readable
- [`43_check_writable.c`](#43-check-writable) - Check writable
- [`45_get_hardlink_count.c`](#45-hardlink-count) - Hardlink count

## 4. Special Files <a id="special-files"></a>
- [`22_create_hardlink.c`](#22-hardlink) - Create hardlink
- [`28_create_fifo_myfifo.c`](#28-create-fifo) - Create FIFO
- [`29_read_from_fifo.c`](#29-read-fifo) - Read from FIFO

## 5. I/O Processing <a id="io-processing"></a>
- [`23_read_csv_data.c`](#23-read-csv) - Read CSV
- [`30_truncate_file.c`](#30-truncate) - Truncate file
- [`31_search_string_lines.c`](#31-search-string) - Search string
- [`36_read_first_10_lines.c`](#36-first-10) - First 10 lines
- [`37_reverse_write_input.c`](#37-reverse) - Reverse input
- [`39_read_binary_file.c`](#39-read-binary) - Read binary
- [`44_execute_shell_commands.c`](#44-execute-shell) - Execute shell
- [`46_combine_all_text_files.c`](#46-combine-text) - Combine text
- [`48_get_bytes_data_bin.c`](#48-get-bytes) - Get bytes
- [`52_read_data_txt.c`](#52-read-data) - Read data.txt

## 6. Miscellaneous <a id="miscellaneous"></a>
- [`24_get_cwd_absolute_path.c`](#24-get-cwd) - Get CWD
- [`33_create_empty_file.c`](#33-empty-file) - Create empty
- [`42_move_log_files_to_logs.c`](#42-move-logs) - Move logs
- [`51_check_file_exists.c`](#51-check-exists) - Check exists

---

## Answers <a id="answers"></a>
### File Operations
<a id="01-create-hello"></a>
**01_create_hello_file.c**
```c
#include <fcntl.h>
#include <unistd.h>

int main() {
    int fd = creat("hello.txt", 0644);
    close(fd);
    return 0;
}

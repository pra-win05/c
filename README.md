v
---

## 1️⃣ File Operations

Basic file handling programs using system calls like `open()`, `read()`, `write()`, `stat()`, etc.

<details>
<summary>Click to expand</summary>

- 01_create_hello_file.c  
- 02_read_file_contents.c  
- 04_check_file_exists.c  
- 05_rename_file.c  
- 06_delete_file.c  
- 07_copy_file.c  
- 08_move_file_to_dir.c  
- 10_get_file_size.c  
- 15_count_lines.c  
- 16_append_to_file.c  
- 54_delete_delete_me.c  
- 55_rename_oldname_newname.c  
- 56_copy_file_contents.c  
- 57_move_file_to_backup.c  
- 59_size_of_data_txt.c  
- 61_count_lines_log_txt.c  
- 62_append_goodbye_message.c  

</details>

---

## 2️⃣ Directory Operations

Programs that create, delete, and manipulate directories and their contents recursively.

<details>
<summary>Click to expand</summary>

- 03_create_test_dir.c  
- 11_check_test_dir_exists.c  
- 12_create_backup_parent_dir.c  
- 13_list_all_recursive.c  
- 14_delete_all_files_temp_dir.c  
- 25_size_of_directory.c  
- 27_count_files_images_dir.c  
- 35_recursive_delete_temp.c  
- 38_create_date_named_dir.c  
- 40_largest_file_in_dir.c  
- 47_total_dir_size_recursive.c  
- 49_create_timestamp_dir.c  
- 50_create_documents_dir.c  
- 58_list_all_current_dir.c  
- 60_create_pictures_in_parent.c  

</details>

---

## 3️⃣ File Metadata

Programs to read and modify metadata using `stat()`, `chmod()`, `chown()`, and more.

<details>
<summary>Click to expand</summary>

- 17_set_readonly_permission.c  
- 18_change_ownership.c  
- 19_get_last_modified_time.c  
- 21_check_file_or_dir.c  
- 32_get_file_type.c  
- 34_get_file_permissions.c  
- 41_check_readable.c  
- 43_check_writable.c  
- 45_get_hardlink_count.c  

</details>

---

## 4️⃣ Special Files

Working with hard links, FIFOs (named pipes), and inter-process communication basics.

<details>
<summary>Click to expand</summary>

- 22_create_hardlink.c  
- 28_create_fifo_myfifo.c  
- 29_read_from_fifo.c  

</details>

---

## 5️⃣ Input/Output and Processing

Text/binary I/O operations, command execution, data parsing and file transformation.

<details>
<summary>Click to expand</summary>

- 23_read_csv_data.c  
- 30_truncate_file.c  
- 31_search_string_lines.c  
- 36_read_first_10_lines.c  
- 37_reverse_write_input.c  
- 39_read_binary_file.c  
- 44_execute_shell_commands.c  
- 46_combine_all_text_files.c  
- 48_get_bytes_data_bin.c  
- 52_read_data_txt.c  

</details>

---

## 6️⃣ Miscellaneous Utilities

Misc C programs using system calls related to file paths and logging.

<details>
<summary>Click to expand</summary>

- 24_get_cwd_absolute_path.c  
- 33_create_empty_file.c  
- 42_move_log_files_to_logs.c  
- 51_check_file_exists.c  

</details>

---

## 📚 How to Compile

You can compile any `.c` file using `gcc`:

```bash
gcc 1_file_operations/01_create_hello_file.c -o create_hello_file
./create_hello_file


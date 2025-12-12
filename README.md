import os
import shutil
import time

def backup_files(source_dir, backup_dir):
    # Get the current date and time to create a timestamped folder
    timestamp = time.strftime("%Y%m%d_%H%M%S")
    backup_folder = os.path.join(backup_dir, f"backup_{timestamp}")
    
    # Create the backup folder if it doesn't exist
    if not os.path.exists(backup_folder):
        os.makedirs(backup_folder)
    
    # Walk through the source directory and copy files to the backup folder
    for root, dirs, files in os.walk(source_dir):
        # Create the same folder structure in the backup directory
        backup_subfolder = os.path.join(backup_folder, os.path.relpath(root, source_dir))
        if not os.path.exists(backup_subfolder):
            os.makedirs(backup_subfolder)
        
        for file in files:
            source_file = os.path.join(root, file)
            backup_file = os.path.join(backup_subfolder, file)
            
            # If the file doesn't already exist in the backup, copy it
            if not os.path.exists(backup_file):
                shutil.copy2(source_file, backup_file)
                print(f"Backed up: {source_file} -> {backup_file}")
            else:
                print(f"Skipped: {source_file} (already exists in backup)")

# Example usage
if __name__ == "__main__":
    source_directory = "C:/path/to/your/source_directory"  # Replace with your source directory path
    backup_directory = "C:/path/to/your/backup_directory"  # Replace with your backup directory path
    
    # Backup files from source to backup
    backup_files(source_directory, backup_directory)

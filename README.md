General outline of the script as I intend it to be.

csvSplit outline:

Create a settings/config file that holds
    default working folder
    app names and API keys

Process outline:
    check for settings file, if none create one.
    if working folder is not specified in settings or not accessable prompt user to select working folder (wf)
        if does not exist, create it
    prompt user for project name (pn)
    if folder exists in working folder with project name (wf/pn)
        if folder named "original" does not exist (wf/pn/original)
            create folder name "original"
        else:
            if 1 CSV file exists in original, set that file as origin file
        if folder name "result" exists (wf/pn/result)
            if wf/pn/result contains files
                prompt user to delete files or create zip of files in wf/pn
    if origin file is not set
        prompt user to select if origin is local or remote
            if local:
                prompt user to select file
                copy file to wf/pn/original
                set copied file as original file
            if remote:
                prompt user for URL of original file
                copy file to wf/pn/original
                set copied file as original file
    copy origin file to wf/pn/result
    set copy as working file
    prompt user to select truncate columns or break into smaller files or upload directly
    if truncate colums
        display column names with check boxes, largest size of information in column, number of rows that do not have data, number of rows that do have data
        prompt user to select columns they wish to be in output
        truncate columns into file named "truncated"filename
    if split into smaller files:
        display file stats:
            largest single row
            average row length
            number of rows
            number of columns
        prompt user to select either number of files or maximum file size
            if maximum file size
                if largest single row + column header will not fit into file size
                    notify user
                    return to truncate/upload/split selection
                copy header column
                while rows exist in working file
                    create new file
                    set current filesize=header column size
                    write header column
                    while current filesize + next line size < max file size:
                        current filesize += next line size
                        write next line to current file
                    close file
            if number of files:
                do current process
    if upload directly:
        prompt for appname
        if apikey not saved in settings file
            apikey=''
        else:
            apikey = saved data
        while unable to access due to apikey:
            prompt for apikey
            save apikey to settings file
        load custom fields
        display list of columns from csv file
        attempt to match to fields/custom fields in application
        allow user to finish matching
        --NEXT
        allow user to match values to currently existing values (tags, phone types, fields that have selections)
        --NEXT
        check for email status
        --NEXT
        display general overview
        --NEXT
        start moderated import of contacts
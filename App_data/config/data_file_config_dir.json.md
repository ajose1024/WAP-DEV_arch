WAP -- WEB Application Platform
-------------------------------

### File:   data_files_config_dir.json


This file holds information a general directory for the GROUP, SECTION, TYPE and ITEM for all elements stored through the WAP_storage class.

It holds a numeric array with ALL elements presently stored and an associative array with the stored element keys, for each stored data element.

<b>Its general format is as follows: (<i>a pratical example with some data</i>)</b>


    {
        "framework": "WEB Application Platform" ,
        "name": "data_file_config_dir" ,
        "vers": "1.0" ,
        "WAP_storage":
        {
            "sections":
            [
                {
                    "section":  "general" ,
                    "type":     "system" ,
                    "item":     "config",
                    "group":    "config_files"
                },
                {
                    "section":  "general" ,
                    "type":     "system" ,
                    "item":     "data_files" ,
                    "group":    "config_files"
                },
                {
                    "section":  "general" ,
                    "type":     "database" ,
                    "item":     "sys_DBs" ,
                    "group":    "config_files"
                } ,
                {
                    "section":   "general" ,
                    "type":     "database" ,
                    "item":     "app_DBs" ,
                    "group":    "config_files"
                } ,
                {
                    "section":  "system" ,
                    "type":     "data_context" ,
                    "item":     "global" ,
                    "group":    "config_files"
                } ,
                {
                    "section":  "resources" ,
                    "type":     "scripts" ,
                    "item":     "app_start" ,
                    "group":    "config_files"
                } ,
                {
                    "section":  "resources" ,
                    "type":     "css" ,
                    "item":     "" ,
                    "group":    "config_files"
                } ,
                {
                    "section":  "resources" ,
                    "type":     "images" ,
                    "item":     "background" ,
                    "group":    "config_files"
                } ,
                {
                    "section":  "resources" ,
                    "type":     "images" ,
                    "item":     "lang_flags" ,
                    "group":    "config_files"
                } ,
                {
                    "section":  "resources" ,
                    "type":     "images" ,
                    "item":     "logos" ,
                    "group":    "config_files"
                } ,
                {
                    "section":  "app_data" ,
                    "type":     "data" ,
                    "item":     "clientes" ,
                    "group":    "config_files"
                } ,
                {
                    "section":  "app_data" ,
                    "type":     "data" ,
                    "item":     "parceiros" ,
                    "group":    "config_files"
                } ,
                {
                    "section":  "app_data" ,
                    "type":     "data" ,
                    "item":     "materiais" ,
                    "group":    "config_files"
                } ,
                {
                    "section":  "app_data" ,
                    "type":     "data" ,
                    "item":     "ext_links" ,
                    "group":    "config_files"
                }
            ]
        }
    }

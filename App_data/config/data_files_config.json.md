WAP -- WEB Application Platform
-------------------------------

### File:   data_files_config.json


This file holds information for the data files stored through the WAP_storage class.

Each entry points to a stored data item, of a given type, specified in the 'mime' field.

Each stored data item can either be STATIC, in which case it will have just only one instance, or CONTEXT, where the same stored item has multiple instances, defined by the 'context' field.

<b>Its general format is as follows: (<i>a pratical example with some data</i>)</b>


    {
        "framework": "WEB Application Platform" ,
        "name": "data_files_config" ,
        "vers": "1.0",
        "data_files_config":
        {
            "config_files":
            {
                "general":
                {
                    "system":
                    {
                        "config":
                        {
                            "base":     "/App_data/config" ,
                            "file":     "/general_config.json" ,
                            "type":     "static" ,
                            "mime":     "text/json" ,
                            "context":  "" ,
                            "handler":  ""
                        },
                        "data_files":
                        {
                            "base":     "/App_data/config" ,
                            "file":     "/data_files_config.json" ,
                            "type":     "static" ,
                            "mime":     "text/json" ,
                            "context":  "" ,
                            "handler":  ""
                        }
                    } ,
                    "database":
                    {
                        "sys_DBs":
                        {
                            "base":     "/App_data/config/databases/" ,
                            "file":     "/sys_DBs.json" ,
                            "type":     "static" ,
                            "mime":     "text/json" ,
                            "context":  "" ,
                            "handler":  ""
                        },
                        "app_DBs":
                        {
                            "base":     "/App_data/config/databases/" ,
                            "file":     "/app_DBs.json" ,
                            "type":     "static" ,
                            "mime":     "text/json" ,
                            "context":  "" ,
                            "handler":  ""
                        }
                    }
                } ,
                "system":
                {
                    "data_context":
                    {
                        "global":
                        {
                            "base":     "/App_data/system/data_context" ,
                            "file":     "/data_context_IDs.json" ,
                            "type":     "static" ,
                            "mime":     "text/json" ,
                            "context":  "" ,
                            "handler":  ""
                        }
                    }
                } ,
                "resources":
                {
                    "scripts":
                    {
                        "app-start":
                        {
                            "base":     "/App_data/resources/scripts/data_context" ,
                            "file":     "/app-start_data.js" ,
                            "type":     "context" ,
                            "mime":     "text/javascript" ,
                            "context":  "" ,
                            "handler":  ""
                        }
                    } ,
                    "css":
                    {
                        "NULL":
                        {
                            "base":     "" ,
                            "file":     "" ,
                            "type":     "" ,
                            "mime":     "" ,
                            "context":  "" ,
                            "handler":  ""
                        }
                    } ,
                    "images":
                    {
                        "background":
                        {
                            "base":     "/App_data/resources/images/background" ,
                            "file":     "/background_images.json" ,
                            "type":     "static" ,
                            "mime":     "text/json" ,
                            "context":  "" ,
                            "handler":  ""
                        } ,
                        "banners":
                        {
                            "base":     "/App_data/resources/images/banners" ,
                            "file":     "/banner_images.json" ,
                            "type":     "static" ,
                            "mime":     "text/json" ,
                            "context":  "" ,
                            "handler":  ""
                        } ,
                        "lang_flags":
                        {
                            "base":     "/App_data/resources/images/lang_flags" ,
                            "file":     "/language_flags.json" ,
                            "type":     "static" ,
                            "mime":     "text/json" ,
                            "context":  "" ,
                            "handler":  ""
                        } ,
                        "logos":
                        {
                            "base":     "/App_data/resources/images/logos" ,
                            "file":     "/social_logos.json" ,
                            "type":     "static" ,
                            "mime":     "text/json" ,
                            "context":  "" ,
                            "handler":  ""
                        }
                    }
                } ,
                "app_data":
                {
                    "data":
                    {
                        "clientes":
                        {
                            "base":     "/App_data/app_data/data/" ,
                            "file":     "/clientes_data.json" ,
                            "type":     "static" ,
                            "mime":     "text/json" ,
                            "context":  "" ,
                            "handler":  ""
                        } ,
                        "parceiros":
                        {
                            "base":     "/App_data/app_data/data/" ,
                            "file":     "/parceiros_data.json" ,
                            "type":     "static" ,
                            "mime":     "text/json" ,
                            "context":  "" ,
                            "handler":  ""
                        } ,
                        "materiais":
                        {
                            "base":     "/App_data/app_data/data/" ,
                            "file":     "/materiais_data.json" ,
                            "type":     "static" ,
                            "mime":     "text/json" ,
                            "context":  "" ,
                            "handler":  ""
                        } ,
                        "ext_links":
                        {
                            "base":     "/App_data/app_data/data/" ,
                            "file":     "/ext_links_data.json" ,
                            "type":     "static" ,
                            "mime":     "text/json" ,
                            "context":  "" ,
                            "handler":  ""
                        }
                    }
                }
            }
        }
    }

WAP -- WEB Application Platform
-------------------------------

## File:   general_config.json


This file holds information for the configuration path for several commonly used config files.

It is automatically created from the 'config_files' group in the 'data_config_files.json' master configuration file.

<b>Its general format is as follows: (<i>a pratical example with some data</i>)</b>




{
    "wap": "WEB Application Platform" ,
    "general_config":
    {
        "config_files":
        {
            "MVC":
            {
                "autoload":         "/App_data/config/MVC/config/autoload.json" ,
                "config":           "/App_data/config/MVC/config/config.json" ,
                "constants":        "/App_data/config/MVC/config/constants.json" ,
                "database":         "/App_data/config/MVC/config/database.json" ,
                "doctypes":         "/App_data/config/MVC/config/doctypes.json" ,
                "foreign_chars":    "/App_data/config/MVC/config/foreign_chars.json" ,
                "hooks":            "/App_data/config/MVC/config/hooks.json" ,
                "migration":        "/App_data/config/MVC/config/migration.json" ,
                "mimes":            "/App_data/config/MVC/config/mimes.json" ,
                "profiler":         "/App_data/config/MVC/config/profiles.json" ,
                "routes":           "/App_data/config/MVC/config/routes.json" ,
                "smileys":          "/App_data/config/MVC/config/smileys.json" ,
                "user_agents":      "/App_data/config/MVC/config/user_agents.php"
            },
            "databases":
            {
                "sys_DBs":          "/App_data/config/databases/sys_DBs.json",
                "app_DBs":          "/App_data/config/databases/app_DBs.json"
            }
        }
    }
}

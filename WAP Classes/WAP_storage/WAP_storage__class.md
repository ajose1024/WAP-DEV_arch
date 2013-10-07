WAP -- WEB Application Platform
-------------------------------

### WAP_config  (static class)


#### Class Interface


>     interface WAP_storage__Interface
>     { 
>         public static function get_version() ;
> 
>     	  public static function get_config_data_file_path( $section, $type, $item, $context = '' ) ;
>     	  public static function get_config_data( $section, $type, $item, $context = '' ) ;
> 	  public static function set_config_data( $data, $section, $type, $item, $context = '' ) ;
>     
>     }


#### Class properties

##### <i>PRIVATE properties:</i>

>     // ------------------------
>     // Class private properties
>     // ------------------------
> 
>     // This property indicates if the calss has been already intitialized or not
>     private static $initialized = FALSE ;
>     
>     // This property holds the version of this static class
>     private static $version = '0.010' ;
>  
>     
>     // Data Files storage: main global property
>     private static $data_files_config = array() ;
>     
>     
>     // This property holds the diferent section's data, indexed by section name
>     //
>     // The section names are as follows:
>     //  'general'   --> This section holds general configuration files in JSON
>     //                  format
>     //  'system'    --> This section holds system configuration files in JSON
>     //                  format
>     //  'resources' --> This section holds resources data channel configuration
>     //                  and data files in JSON format
>     //                  As sub-types, it has:
>     //                  'scripts'   --> Javascript scripts
>     //                  'css'       --> CSS files
>     //                  'images'    --> Image files, of different types, and for
>     //                                  different functions and contexts
>     private static $data_sections_data = array() ;
>     
> 


##### <i>PUBLIC properties:</i>

>     // -----------------------
>     // Class public properties
>     // -----------------------
> 


#### Class methods

##### <i>PRIVATE methods:</i>

>     // Method:    __construct()       // Private method
>     //
>     // Construct won't be called inside this class and is uncallable from
>     // the outside.
>     // This prevents instantiating this class.
>     // This is by purpose, because we want a static class.
>     
>     private function __construct()
>     {
>         
>     }
> 
> 
>     // Method:  initialize()        // Private method
>     //
>     // Class private initialize function
>     //
>     // This class initializes the private  self::$initialized  property to TRUE
>     // and performs the necessary initialization functions
> 
>     private static function initialize()
>     {
>         // Test if  self::$initialized  is set to TRUE and return if so
>         if ( self::$initialized === TRUE )
>         {
>             return ;
>         }
> 
>         // Set  self::$initialized  to TRUE to indicate initialization already performed
>         self::$initialized = TRUE ;
>
>         // ----------------------------
>         // Perform class INITIALIZATION
>         // ----------------------------
>         //
>         // Load and de-serialize the 'data_files_config.json' file
>         $data_files_config_file = $GLOBALS[ 'DOC_ROOT' ] . 
>                                   "/App_data/config/data_files_config.json"
>                                 ;
>         
>         self::$data_files_config = object_to_array( read_JSON_file( $data_files_config_file ) ) ;
>         
>         // Get the 'config_files' data section
>         $config_files = self::$data_files_config[ 'data_files_config' ][ 'config_files' ] ;
>         
>         foreach( $config_files as $section_name => $section_data )
>         {
>             
>             self::$data_sections_data = array_merge( self::$data_sections_data , 
>                                                      array( $section_name => $section_data )
>                                                    ) ;
>         }
>     }



##### <i>PUBLIC methods:</i>

>     // Method:  get_version()       // Public method
>     //
>     // This method returns the version of the WAP_data_context static class
> 
>     public static function get_version()
>     {
>         self::initialize() ;
>         return self::$version ;
>     }
> 
> 
>     // Method:  get_config_data_file_path( $section , $type , $item, $context = '' )
>     //
>     // This method returns the config data file path related to the given item:
>     //
>     //  SECTION:    This selects the desired section ('general', 'resources', 
>     //              etc. )
>     //  TYPE:       This selects the TYPE inside the selected SECTION, such as,
>     //              for 'resources' section:
>     //                  'scripts'
>     //                  'css'
>     //                  'images'
>     //                  etc ...
>     //  ITEM:       This selects the specific element, inside a SECTION and
>     //              within a TYPE, such as, for 'images' type:
>     //                  back_ground
>     //                  banners
>     //                  lang_files
>     //                  logos
>     //
>     // If the selected config data does not exist, then just returns NULL.
>     //
>     // If it exists, it returns an array with the following format:
>     //
>     //  return_data ->  array( 'file_path'  =>  '/path/to/the/selected/file' ,
>     //                         'mime-type'  =>  'file_mime-type'
>     //                       )
>     
>     public static function get_config_data_file_path( $section, $type, $item, $context = '' )
>     {        
>         if( key_exists( $section, self::$data_sections_data ) )
>         {
>             if( key_exists( $type, self::$data_sections_data[ $section ] ) )
>             {
>                 if( key_exists( $item, self::$data_sections_data[ $section ][ $type ] ) )
>                 {
>		      // ----------------------------
>                     // Get the config file location
>		      // ----------------------------
>
>		      // Pre-load the 'document_root' base path
>                     $config_file_path = $GLOBALS[ 'DOC_ROOT' ] ;
>                     
>                     if( key_exists( 'type', self::$data_sections_data[ $section ][ $type ][ $item ] ) )
>                     {
>                         if( key_exists( 'base', self::$data_sections_data[ $section ][ $type ][ $item ] ) )
>                         {
>                             if( key_exists( 'file', self::$data_sections_data[ $section ][ $type ][ $item ] ) )
>                             {
>                                 switch ( self::$data_sections_data[ $section ][ $type ][ $item ][ 'type' ] )
>                                 {        
>                                     case 'static':
>                                         // The file type is 'static', so it does not have any 'context_ID'
>                                         // in it's path
>                                         $config_file_path = $config_file_path .
>                                                             self::$data_sections_data[ $section ][ $type ][ $item][ 'base' ] .
>                                                             self::$data_sections_data[ $section ][ $type ][ $item][ 'file' ] ;
>                                         break ;
>                                 
>                                     case 'context':
>                                         // The file type is 'context', so it does have the 'context_ID'
>                                         // between the 'file_base' and the 'file_name'
>                                         if( $context === '' )
>                                         {
>                                             $context = '' ;
>                                         }
>                                         else
>                                         {
>                                             $context = '/' . $context ;
>                                         }
>                                         $config_file_path = $config_file_path .
>                                                             self::$data_sections_data[ $section ][ $type ][ $item][ 'base' ] .
>                                                             $context .
>                                                             self::$data_sections_data[ $section ][ $type ][ $item][ 'file' ] ;
>                                         break ;
>                                 
>                                     DEFAULT:
>                                         // Handle the remaining options
>                                         
>                                         break ;
>                                 }
>                                 
>                                 // Return the file path
>                                 
>                                 if( key_exists( 'mime', self::$data_sections_data[ $section ][ $type ][ $item ] ) )
>                                 {
>                                     $mime_type = self::$data_sections_data[ $section ][ $type ][ $item ][ 'mime' ] ;
>                                 }
>                                 else
>                                 {
>                                     $mime_type = '' ;
>                                 }
>                                 
>                                 return  array( 'file_path' => $config_file_path ,
>                                                'mime-type' => $mime_type
>                                              ) ;
>                             }
>                             else
>                             {
>                                 // -------------------------------------------
>                                 // Return NULL as the file name is not defined
>                                 // -------------------------------------------
>                                 return  NULL ;
>                             }
>                         }
>                         else
>                         {
>                             // ------------------------------------------------
>                             // Return NULL as the file base path is not defined
>                             // ------------------------------------------------
>                             return  NULL ;
>                         }
>                     }
>                     else
>                     {
>                         // --------------------------------------------------
>                         // Return NULL as the type of the file is not defined
>                         // --------------------------------------------------
>                         return  NULL ;
>                     }
>                 }
>                 else
>                 {
>                     // ---------------------------
>                     // Exists   'SECTION' & 'TYPE'
>                     // Lacks    'ITEM'
>                     // ---------------------------
>                     return  NULL ;
>                 }
>             }
>             else
>             {
>                 // ------------------
>                 // Exists   'SECTION'
>                 // Lacks    'TYPE'
>                 // Untested 'ITEM'
>		  // ------------------
>                 return  NULL ;
>             }
>         }
>         else
>         {
>             // ------------------------
>             // Lacks    'SECTION'
>             // Untested 'TYPE' & 'ITEM'
>             // ------------------------
>             return  NULL ;
>         }
>         
>     }
>     
>     
>     // Method:  get_config_data( $section, $type, $item, $context = '' )
>     //
>     // This method returns the config data file path related to the given item:
>     //
>     //  SECTION:    This selects the desired section ('general', 'resources', 
>     //              etc. )
>     //  TYPE:       This selects the TYPE inside the selected SECTION, such as,
>     //              for 'resources' section:
>     //                  'scripts'
>     //                  'css'
>     //                  'images'
>     //                  etc ...
>     //  ITEM:       This selects the specific element, inside a SECTION and
>     //              within a TYPE, such as, for 'images' type:
>     //                  back_ground
>     //                  banners
>     //                  lang_files
>     //                  logos
>     //
>     // If the selected config data does not exist, then just returns NULL.
> 
>     public static function get_config_data( $section, $type, $item, $context = '' )
>     {
>         // -------------------------------------------
>	  // Get the config data file path and file type
>	  // -------------------------------------------
>
>         $config_file_path_data = self::get_config_data_file_path( $section, $type, $item, $context ) ;
>
>	  // ----------------
>	  // Process the file
>	  // ----------------
>         
>         if( ! is_null( $config_file_path_data ) )
>         {
>             if( key_exists( 'file_path', $config_file_path_data ) )
>             {
>                 if( file_exists( $config_file_path_data[ 'file_path' ] ) )
>                 {
>                     if( key_exists( 'mime-type', $config_file_path_data ) )
>                     {
>                         $mime_type = $config_file_path_data[ 'mime-type' ] ;
>                         
>                         switch( $mime_type )
>                         {
>			      // ----------------------------------------
>                             // The file type is 'text/json' (JSON file)
>                             // ----------------------------------------
>                             case 'text/json':
>                                 return  object_to_array( read_JSON_file( $config_file_path_data[ 'file_path' ] ) ) ;
>                                 
>                                 break ;
>                             
>			      // --------------------------------------------
>			      // The file type is 'text/javascript' (JS file)
>			      // --------------------------------------------
>                             case 'text/javascript':
>                                 return  file_get_contents( $config_file_path_data[ 'file_path' ], FALSE ) ;
>                             
>                                 break ;
>                             
>			      // ------------------------
>			      // The file type in unknown
>                             // ------------------------
>                             default:
>                                 return  NULL ;
>                                 
>                                 break ;
>                         }
>                     }
>                     else
>                     {
>			  // -------------------------------------------------
>			  // The file path data 'mime-type' key does not exist
>			  // -------------------------------------------------
>                         return  NULL ;
>                     }
>                 }
>                 else
>                 {
>		      // -----------------------
>                     // The file does not exist
>                     // -----------------------
>                     return  NULL ;
>                 }
>             }
>             else
>             {
>	  	  // -------------------------------------------------
>		  // The file path data 'file-path' key does not exist
>		  // -------------------------------------------------
>                 return  NULL ;
>             }
>         }
>         else
>         {
>	      // ---------------------------------------------------------------
>	      // The file parameters ($section, $type, $item, $context) does not
>	      // correspond to any defined file
>	      // ---------------------------------------------------------------
>             return  NULL ;
>         }
>     }
>     
>     
>     // Method:  set_config_data( $data, $section, $type, $item, $context = '' )
>     //
>     // This method stores the config data file path related to the given item:
>     //
>     //  SECTION:    This selects the desired section ('general', 'resources', 
>     //              etc. )
>     //  TYPE:       This selects the TYPE inside the selected SECTION, such as,
>     //              for 'resources' section:
>     //                  'scripts'
>     //                  'css'
>     //                  'images'
>     //                  etc ...
>     //  ITEM:       This selects the specific element, inside a SECTION and
>     //              within a TYPE, such as, for 'images' type:
>     //                  back_ground
>     //                  banners
>     //                  lang_files
>     //                  logos
>     //
>     // The data file is passed in the $data parameter
> 
>     public static function set_config_data( $data, $section, $type, $item, $context = '' )
>     {
>         $config_file_path_data = self::get_config_data_file_path( $section, $type, $item, $context ) ;
>         
>         if( ! is_null( $config_file_path_data ) )
>         {
>             if( key_exists( 'file_path', $config_file_path_data ) )
>             {
>                 if( file_exists( $config_file_path_data[ 'file_path' ] ) )
>                 {
>                     if( key_exists( 'mime-type', $config_file_path_data ) )
>                     {
>                         $mime_type = $config_file_path_data[ 'mime-type' ] ;
>                         
>                         switch( $mime_type )
>                         {
>                             case 'text/json':
>                                 write_JSON_file( $config_file_path_data[ 'file_path' ] , $data ) ;
> 
>                                 break ;
>                             
>                             case 'text/javascript':
>                                 file_put_contents( $config_file_path_data[ 'file_path' ], $data ) ;
>                             
>                                 break ;
>                             
>                             default:
>                                 return  NULL ;
>                                 
>                                 break ;
>                         }
>                     }
>                     else
>                     {
>                         return  NULL ;
>                     }
>                 }
>                 else
>                 {
>                     return  NULL ;
>                 }
>             }
>             else
>             {
>                 return  NULL ;
>             }
>         }
>         else
>         {
>             return  NULL ;
>         }
>     }
>     
>     
> 
>     
>     
>     public static function dump_data_files_config()
>     {
>         return  self::$data_files_config ;
>     }
>     
>     public static function dump_data_section_data()
>     {
>         return  self::$data_sections_data ;
>     }



#### Class inicialization

>     $GLOBALS[ 'WAP_config' ] = WAP_config::get_version() ;
>     $WAP_data::$module_version[ 'WAP_config' ] = WAP_config::get_version() ;


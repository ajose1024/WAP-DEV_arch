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
>         self::load_data_files_config() ;
>         
>         // Get the 'config_files' data section
>         self::read_data_sections_data() ;
>     }
>     
>     
>     // Method:  load_data_files_config()        // Private method
>     //
>     // This method loads and unserialises the following JSON file:
>     //
>     //  '/App_data/config/data_files_config.json'
>     //
>     // onto the private property  self::$data_files_config
>     
>     private static function load_data_files_config()
>     {
>         $data_files_config_file = $GLOBALS[ 'DOC_ROOT' ] . 
>                                   "/App_data/config/data_files_config.json"
>                                 ;
>         
>         self::$data_files_config = object_to_array( read_JSON_file( $data_files_config_file ) ) ;
>         
>     }
>     
>     
>     // Method:  save_data_files_config()        // Private method
>     //
>     // This method serializes and saves the following private property
>     //
>     //  self::$data_files_config
>     //
>     // onto the followinf JSON file:
>     //
>     //  '/App_data/config/data_files_config.json
>     
>     private static function save_data_files_config()
>     {
>         $data_files_config_file = $GLOBALS[ 'DOC_ROOT' ] . 
>                                   "/App_data/config/data_files_config.json"
>                                 ;
> 
>         write_JSON_file( $data_files_config_file , self::$data_files_config ) ;
>     }
>     
> 
>     // Method:  read_data_sections_data()
>     //
>     // This method reads the 'config_files' section from the  self::$data_files_config
>     // array into the  self::$data_sections_data  array
>     
>     private static function read_data_sections_data()
>     {
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
>     
>     
>     // Method:  update_data_sections_data()
>     //
>     // This method updates the  self::#data_sections_data  associative array onto the
>     // 'config_files' section of the  self::$data_files_config  associative array
>     
>     private static function update_data_sections_data()
>     {
>         self::$data_files_config[ 'data_files_config' ] = array_merge(
>                                         self::$data_files_config[ 'data_files_config' ] ,
>                                         array( 'config_files' => self::$data_sections_data ) 
>                                                                      ) ;
>     }
>     
>     
> 
 

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
>                     // Get the config file location
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
>                                 // --------------------                                
>                                 // Return the file path
>                                 // --------------------
>                                 
>                                 if( key_exists( 'mime', self::$data_sections_data[ $section ][ $type ][ $item] ) )
>                                 {
>                                     $mime_type = self::$data_sections_data[ $section ][ $type ][ $item][ 'mime' ] ;
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
>                                 // Return NULL as the file name is not defined
>                                 return  NULL ;
>                             }
>                         }
>                         else
>                         {
>                             // Return NULL as the file base path is not defined
>                             return  NULL ;
>                         }
>                     }
>                     else
>                     {
>                         // Return NULL as the type of the file is not defined
>                         return  NULL ;
>                     }
>                 }
>                 else
>                 {
>                     // Exists   'SECTION' & 'TYPE'
>                     // Lacks    'ITEM'
>                     return  NULL ;
>                 }
>             }
>             else
>             {
>                 // Exists   'SECTION'
>                 // Lacks    'TYPE'
>                 // Untested 'ITEM'
>                 return  NULL ;
>             }
>         }
>         else
>         {
>             // Lacks    'SECTION'
>             // Untested 'TYPE' & 'ITEM'
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
>                                 return  object_to_array( read_JSON_file( $config_file_path_data[ 'file_path' ] ) ) ;
>                                 
>                                 break ;
>                             
>                             case 'text/javascript':
>                                 return  file_get_contents( $config_file_path_data[ 'file_path' ], FALSE ) ;
>                             
>                                 break ;
>                             
>                             case 'plain/text':
>                                 return  file_get_contents( $config_file_path_data[ 'file_path' ], FALSE ) ;
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
>     public static function set_config_data( $data,
>                                             $section, $type, $item,
>                                             $context = '' ,
>                                             $file_data = array( 'base' => '/App_data/tmp/' ,
>                                                                 'file' => 'null-file.dat',
>                                                                 'type' => 'static' ,
>                                                                 'mime' => 'plain/text'
>                                                               )
>                                           )
>     {
>         $config_file_path_data = self::get_config_data_file_path( $section, $type, $item, $context ) ;
>         
>         if( ! is_null( $config_file_path_data ) )
>         {
>             // ------------------------------
>             // Test is 'file_path' key exists
>             // ------------------------------
>             if( key_exists( 'file_path', $config_file_path_data ) )
>             {
>                 
>                 if( key_exists( 'mime-type', $config_file_path_data ) )
>                 {
>                     $mime_type = $config_file_path_data[ 'mime-type' ] ;
>                     
>                     var_dump( $config_file_path_data ) ;
>                     
>                     switch( $mime_type )
>                     {
>                         case 'text/json':
>                             write_JSON_file( $config_file_path_data[ 'file_path' ] , $data ) ;
> 
>                             break ;
>                             
>                         case 'text/javascript':
>                             file_put_contents( $config_file_path_data[ 'file_path' ], $data ) ;
>                         
>                             break ;
>                             
>                         case 'plain/text':
>                             file_put_contents( $config_file_path_data[ 'file_path' ], $data ) ;
>                         
>                             break ;
>                             
>                         default:
>                             return  NULL ;
>                             
>                             break ;
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
>     // Method:  create_config_data( $data, $section, $type, $item, $context = '',
>     //                              file_data = array( 'base' => '' ,
>     //                                                 'file' => '' ,
>     //                                                 'type' => '' ,
>     //                                                 'mime' => ''
>     //                                               )
>     //                            )
>     //
>     // This method stores the config data file path related to the given item,
>     // creating the required file.
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
>     // The  $file_data  parameter is an array with the following structure:
>     // 
>     //  file_data = array( 'base' => '' ,
>     //                     'file' => '' ,
>     //                     'type' => '' ,
>     //                     'mime' => ''
>     //                   )
>     //
>     // If the file can be successfully created and its data added to the 
>     // self::$data_files_config, the method returns TRUE.
>     // 
>     // Otherwise, it returns FALSE.
>     
>     public static function create_config_data( $data = '' ,
>                                                $section = '' , $type = '' , $item = '' ,
>                                                $context = '' ,
>                                                $file_data = array( 'base' => '/App_data/tmp/data/' ,
>                                                                    'file' => 'tmp-file.dat',
>                                                                    'type' => 'static' ,
>                                                                    'mime' => 'plain/text'
>                                                                  )
>                                              )
>     {
>         // ---------------------------------------------------
>         // Adjust  $file_data  for the required default values
>         // ---------------------------------------------------
>         
>         // Adjust 'default' random file name
>         $file_data[ 'file' ] = str_replace( '/', '|', mk_rnd_akey( 16 ) . '.dat' ) ;
>         
>         // Adjust the file type, according to the $context parameter
>         if( $context === '' )
>         {
>             $file_data[ 'type' ] = 'static' ;
>         }
>         else
>         {
>             $file_data[ 'type' ] = 'context' ;
>         }
>         
>         // Test if all 3 sections are defined or not
>         if( $section !== '' && $type !== '' && $item !== '' )
>         {
>             // If ALL  $section, $type, $item  are defined and diffent from ''
>             self::$data_sections_data = array_merge( self::$data_sections_data ,
>                                                      array( $section => array( $type => array( $item => $file_data
>                                                                                              )
>                                                                              )
>                                                           )
>                                                    ) ;
>         }
>         else
>         {
>             // One or more of the file descriptors is not defined, so the file
>             // can not be successfully created and stored.
>             //
>             // Signal this condition by returning FALSE
>             
>             return  FALSE ;
>         }
>         
>         // Make sure that the actual 'base' directory exists...
>         $file_base_dir = $GLOBALS[ 'DOC_ROOT' ] . $file_data[ 'base' ] ;
>         
>         if( !file_exists( $file_base_dir ) )
>         {
>             mkdir( $file_base_dir, 0750 , TRUE ) ;
>         }
>         
>         // Then call  self::set_config_data($data, $section, $type, $item)
>         // to actualy store the contents
>         self::set_config_data( $data, $section, $type, $item, $context, $file_data ) ;
>         
>         self::update_data_sections_data() ;
>         self::save_data_files_config() ;
>         
>         return  TRUE ;
>     }
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

>     $GLOBALS[ 'WAP_storage' ] = WAP_storage::get_version() ;
>     $WAP_data::$module_version[ 'WAP_storage' ] = WAP_storage::get_version() ;


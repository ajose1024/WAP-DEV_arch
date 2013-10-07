WAP -- WEB Application Platform
-------------------------------

### WAP_config  (static class)


#### Class Interface


>     interface WAP_storage__Interface
>     { 
>         public static function get_version() ;
> 
>         public static function get_data( $group , $file , $item ) ;
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


> 
> 
>     // Method:  get_user_property( $property )    // Private method
>     //
>     // This function gets the value on the static property in this class, whose
>     // name is passed in $property
> 
>     private static function get_user_property( $property )
>     {
>         if( ! property_exists( 'WAP_config', $property ) )
>         {
>             return  null ;
>         }
> 
>         $vars = get_class_vars( 'WAP_config' ) ;
>         return  $vars[ $property ] ;
>     }
> 
> 
>     // Method:  set_user_property( $property , $value )       // Private method
>     //
>     // This function sets the value on the static property in this class, whose
>     // name is passed in $property with the value passed in $value
> 
>     private static function set_user_property( $property , $value )
>     {
>         if( ! property_exists( 'WAP_config' , $property ) )
>         {
>             return  false ;
>         }
>         
>         // Since I cannot trust the value of $value
>         // I am putting it in single quotes (I don't
>         // want its value to be evaled. Thus it will
>         // just be parsed as a variable reference)
> 
>         eval( 'WAP_config' . '::$' . $property . ' = $value;' ) ;
> 
>         return  true ;
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
>     // Method:  get_data( $group , $file , $item )
>     //
>     // This method retrives the selected data from the private storage properties
>     // using the parameters  $group, $file, $item  to select the data to return
> 
>     public static function get_data( $group , $file , $item )
>     {
>         $data = self::get_user_property( $group . '_' . $file . '_config' ) ;
> 
>         if( key_exists( $item , $data[ $file ] ) )
>         {
>             return  $data[ $file ][ $item ] ;
>         }
>         else
>         {
>             return  '' ;
>         }
>     }
> 
> 
>     // Method:    string_to_boolean( $item, $key )
>     //
>     // This method performs a typecast to BOOLEAN from a STRING variable
>
>     public static function string_to_boolean( $item, $key )
>     {
>         if( strtoupper( $item ) === 'TRUE' )
>         {
>             // The string corresponds to a boolean TRUE
>             // Return a boolean TRUE
>             return  (bool) TRUE ;
>         }
>         elseif( strtoupper( $item ) === 'FALSE' )
>         {
>             // The string corresponds to a boolean FALSE
>             // Return a boolean FALSE
>             return  (bool) FALSE ;
>         }
>         else
>         {
>             // The string does not correspond neither to a boolean TRUE nor to
>             // a boolean FALSE
>             // Return the string itself, unchanged
>             return  $item ;
>         }
>     }
> 
> 
>     // Method:  adjust_boolean_vars( $data_array )
>     //
>     // This function receives an array and recursivelly adjust every string element
>     // with either 'true' or 'false' to the corresponding boolean values of TRUE
>     // or FALSE. Otherwise, the array element is unchanged.
>     //
>     // If the element passed to the method is not an array, the function doesn't
>     // do anything
>     
>     public static function adjust_boolean_vars( $data_array )
>     {
>         if( is_array( $data_array ) )
>         {
>             $ret_val = array_walk_recursive( $data_array ,  array( 'WAP_config' , 'string_to_boolean' ), $parameters = NULL ) ;
> 
>             return  $data_array ;
>         }
>         else
>         {
>             return  $data_array ;
>         }
>     }
>     
> 
>     // Method:  dump_general_config()     // Public method
>     //
>     // This method is a temporary method to be used during development
> 
>     public static function dump_general_config()
>     {
>         return  self::$general_config ;
>     }
    
}


#### Class inicialization

>     $GLOBALS[ 'WAP_config' ] = WAP_config::get_version() ;
>     $WAP_data::$module_version[ 'WAP_config' ] = WAP_config::get_version() ;


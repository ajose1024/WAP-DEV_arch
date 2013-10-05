WAP -- WEB Application Platform
-------------------------------

### WAP_config  (static class)


#### Class Interface


interface WAP_config__Interface
{

    public static function get_version() ;


    
}

class WAP_config implements WAP_config__Interface
{

    /**
     * Construct won't be called inside this class and is uncallable from
     * the outside.
     * This prevents instantiating this class.
     * This is by purpose, because we want a static class.
     */
    private function __construct()
    {
        
    }

    
    // Class private properties

    // This property indicates if the calss has been already intitialized or not
    private static $initialized = FALSE ;
    
    // This property holds the version of this static class
    private static $version = '0.010' ;
 
    
    // This property holds the  'general_config'  file data
    private static $general_config = null ;
    
    
    // -----------------------
    // CodeIgniter config data
    // -----------------------

    // This property holds the  'MVC_autoload_config'  file data
    private static $MVC_autoload_config = array() ;
    
    // This property holds the  'MVC_config_config'  file data
    private static $MVC_config_config = array() ;
    
    // This property holds the  'MVC_constants_config'  file data
    private static $MVC_constants_config = array() ;
    
    // This property holds the  'MVC_database_config'  file data
    private static $MVC_database_config = array() ;
    
    // This property holds the  'MVC_doctypes_config'  file data
    private static $MVC_doctypes_config = array() ;
    
    // This property holds the  'MVC_foreign_chars_config'  file data
    private static $MVC_foreign_chars_config = array() ;
    
    // This property holds the  'MVC_hooks_config'  file data
    private static $MVC_hooks_config = array() ;
    
    // This property holds the  'MVC_migration_config'  file data
    private static $MVC_migration_config = array() ;
    
    // This property holds the  'MVC_mimes_config'  file data
    private static $MVC_mimes_config = array() ;
    
    // This property holds the  'MVC_profiles_config'  file data
    private static $MVC_profiles_config = array() ;
    
    // This property holds the  'MVC_routes_config'  file data
    private static $MVC_routes_config = array() ;
    
    // This property holds the  'MVC_smileys_config'  file data
    private static $MVC_smileys_config = array() ;
    
    // This property holds the  'MVC_user_agents_config'  file data
    private static $MVC_user_agents_config = array() ;
    
    
    
    
    
    
    
    // -----------------------
    // Class public properties
    // -----------------------

    // Class private initialize fuction

    private static function initialize()
    {
        if ( self::$initialized )
        {
            return ;
        }

        self::$initialized = TRUE ;
        
        // Initialize the  WAP_config  class
        $general_config_file = $GLOBALS[ 'DOC_ROOT' ] .
                               "/App_data/config/general_config.json" ;

        if( file_exists( $general_config_file ) )
        {
            self::$general_config = object_to_array( read_JSON_file( $general_config_file ) ) ;
        }
        else
        {
            self::$general_config = array() ;
        }

        
        // Read MVC configuration files
        
        $MVC_config_files = self::$general_config[ 'general_config' ][ 'config_files' ][ 'MVC' ] ; 
        
        foreach( $MVC_config_files as $MVC_config_name => $MVC_config_file )
        {
            if( file_exists( $GLOBALS[ 'DOC_ROOT' ] . $MVC_config_file ) )
            {
                $file_data = object_to_array( read_JSON_file( $GLOBALS[ 'DOC_ROOT' ] . $MVC_config_file ) ) ;
                
//                echo "<h1>BEFORE:</h1>" ;
//                var_dump( $file_data ) ;

//                $ret_val = array_walk_recursive( & $file_data ,  array( 'WAP_config' , 'string_to_boolean' ), $parameters = NULL ) ;
//                $new_file_data = array_map( array( 'WAP_config', 'string_to_boolean' ), $file_data ) ;
                
//                $file_data = self::adjust_boolean_vars( $file_data ) ;
                
//                echo "<h1>AFTER:</h1>" ;
//                var_dump( $new_file_data ) ;
                
                self::set_user_property( 'MVC_' . $MVC_config_name . '_config' , 
                                         $file_data
                                       ) ;
                self::get_user_property( 'MVC_' . $MVC_config_name . '_config' ) ;
            }
        }
        
//        var_dump( self::$MVC_autoload_config ) ;
//        var_dump( self::$MVC_config_config ) ;

    }

    
    // Method:  get_version()
    //
    // This method returns the version of the WAP_data_context static class

    public static function get_version()
    {
        self::initialize() ;
        return self::$version ;
    }

    
    // Method:  get_data( $group , $file , $item )
    //
    // This method retrives the selected data from the private storage properties
    // using the parameters  $group, $file, $item  to select the data to return
    
    public static function get_data( $group , $file , $item )
    {
        $data = self::get_user_property( $group . '_' . $file . '_config' ) ;

        if( key_exists( $item , $data[ $file ] ) )
        {
            return  $data[ $file ][ $item ] ;
        }
        else
        {
            return  '' ;
        }
    }
    
    
    public static function string_to_boolean( $item, $key )
    {
//        echo "<h5>KEY: </h5>" ;
//        var_dump( $key ) ;
//        echo "<h5>ITEM: </h5>" ;
//        var_dump( $item ) ;

        if( strtoupper( $item ) === 'TRUE' )
        {
            // The string corresponds to a boolean TRUE
            // Return a boolean TRUE
            return  (bool) TRUE ;
        }
        elseif( strtoupper( $item ) === 'FALSE' )
        {
            // The string corresponds to a boolean FALSE
            // Return a boolean FALSE
            return  (bool) FALSE ;
        }
        else
        {
            // The string does not correspond neither to a boolean TRUE nor to
            // a boolean FALSE
            // Return the string itself, unchanged
            return  $item ;
        }
    }
    
    
    // Method:  adjust_boolean_vars( $data_array )
    //
    // This function receives an array and recursivelly adjust every string element
    // with either 'true' or 'false' to the corresponding boolean values of TRUE
    // or FALSE. Otherwise, the array element is unchanged.
    //
    // If the element passed to the method is not an array, the function doesn't
    // do anything
    
    public static function adjust_boolean_vars( $data_array )
    {
        if( is_array( $data_array ) )
        {
            $ret_val = array_walk_recursive( $data_array ,  array( 'WAP_config' , 'string_to_boolean' ), $parameters = NULL ) ;

//            var_dump( $ret_val ) ;
            
//            var_dump( $data_array ) ;
            
            return  $data_array ;
        }
        else
        {
            return  $data_array ;
        }
    }
    


    
    // Method:  get_user_property( $property )
    //
    // This function gets the value on the static property in this class, whose
    // name is passed in $property
    
    private static function get_user_property( $property )
    {
        if( ! property_exists( 'WAP_config', $property ) )
        {
            return  null ;
        }

        $vars = get_class_vars( 'WAP_config' ) ;
        return  $vars[ $property ] ;
    }


    // Method:  set_user_property( $property , $value )
    //
    // This function sets the value on the static property in this class, whose
    // name is passed in $property with the value passed in $value

    private static function set_user_property( $property , $value )
    {
        if( ! property_exists( 'WAP_config' , $property ) )
        {
            return  false ;
        }
        
        /* Since I cannot trust the value of $value
         * I am putting it in single quotes (I don't
         * want its value to be evaled. Now it will
         * just be parsed as a variable reference).
         */
        
        eval( 'WAP_config' . '::$' . $property . ' = $value;' ) ;
        
        return  true ;
    }

    
    
    public static function dump_general_config()
    {
        return  self::$general_config ;
    }
    
}


$GLOBALS[ 'WAP_config' ] = WAP_config::get_version() ;

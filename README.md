![build status](https://travis-ci.org/fnordahl/cdbc.svg?branch=master "Build status")
[![Snap Status](https://build.snapcraft.io/badge/fnordahl/cdbc.svg)](https://build.snapcraft.io/user/fnordahl/cdbc)
# CDBC

CDBC is a C library providing simple and easy to use interfaces to the ODBC API

## Example code
    #include "cdbc.h"
    
    int main(int argc, char **argv) {
        CDBC cdbc;
        CDBC_QUERY q;
        CDBC_RESULT res;
        char *row_value;
        int id = 1;
        
        // initialize CDBC and connect
        cdbc = cdbc_init();
        cdbc_connect(cdbc, "MySQL", "DATABASE=mydb", "127.0.0.1", 3306, "username", "password");
        
        // prepare SQL query
        q = cdbc_prepare(cdbc, "SELECT * FROM table WHERE id = ?");
        
        // bind input parameters
        cdbc_bind_param(q, 1, CDBC_C_LONG, id, sizeof(id));
        
        // execute SQL query
        cdbc_execute(q);
        
        // get pointer to row in resultset
        row_value = cdbc_c_name(q, "row_name");
        
        // fetch data
        res = cdbc_fetch(q);
        while (res) {
            printf("%s\n", row_value);
            res = cdbc_fetch(q);
        }
        
        // clean up
        cdbc_finish(q);
        cdbc_disconnect(cdbc);
        cdbc_cleanup(cdbc);
        
        return 0;
    }
    
Have a look at https://github.com/fnordahl/cdbc/blob/master/test.c for more a more complete example with error handling etc.

# Note! This template will be removed on the release of 1.53.0 Oskari.

Use https://github.com/oskariorg/sample-server-extension instead (Doesn't work with 1.52.0 as both have flyway module named "example").

# oskari-server-extension-template

This is a template for extending oskari-server functionality using Maven.

Modify oskari-ext.properties:

    # replace 'sample' with 'myapp'
    db.additional.modules=myplaces, analysis, userlayer, myapp
    
    # default view is the first appsetup
    view.default=1
    
    # publish template is the second appsetup
    view.template.publish=2

    # make myplaces etc baselayers have the correct projection for initial setup:
    oskari.native.srs=EPSG:3067

To enable end-user registration configure these (more information at http://oskari.org/documentation/features/usermanagement):

    allow.registration=true
    oskari.email.sender=<sender@domain.com>
    oskari.email.host=<smtp.domain.com>

## Modifying the initial application setup:
 
1) You will need a Java migration to run your setup file like this:
 
    app-resources/src/main/java/flyway/myapp/V1_0_0__initial_db_content.java

Or you can run these as individual flyway migrations.

*Note!* All database tables are created with the core migration.

2) Your setup should add application specific content like layers, users, appsetups.

You can reference sql files under oskari-server/content-resources like adding inspire-themes:

    app-resources/src/main/resources/setup/myapp.json

You can modify dataproviders and users as this is just another example overriding the sample setup:

    app-resources/src/main/resources/sql

For example if you would like to have a non-admin user you can add these lines to app-resources/src/main/resources/sql/initial-users.sql:

    -- add user;
    INSERT INTO oskari_users(user_name, first_name, last_name, uuid) VALUES('user', 'Oskari', 'Olematon', 'fdsa-fdsa-fdsa-fdsa-fdsa');
    
    -- add role to user;
    INSERT INTO oskari_role_oskari_user(user_id, role_id) VALUES((SELECT id FROM oskari_users WHERE user_name = 'user'), (SELECT id FROM oskari_roles WHERE name = 'User'));
    
    -- add credentials user/user for non-admin user;
    INSERT INTO oskari_jaas_users(login, password) VALUES('user', 'MD5:ee11cbb19052e40b07aac0ca060c23ee');

Layers can be configured in json:

    app-resources/src/main/resources/json/layers

And referenced in appsetups like this:

    app-resources/src/main/resources/json/views/myapp-geoportal.json#L12

Compile with:

    mvn clean install
    
Replace oskari-map.war under {jetty.home}/webapps/ with the one created under webapp-map/target 

# Reporting issues

All Oskari-related issues should be reported here: https://github.com/oskariorg/oskari-docs/issues

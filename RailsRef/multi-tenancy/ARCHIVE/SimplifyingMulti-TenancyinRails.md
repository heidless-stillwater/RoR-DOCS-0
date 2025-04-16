## [Simplifying Multi-Tenancy in Rails](https://medium.com/@kiasalvati/simplifying-multi-tenancy-in-rails-comparing-rubhttps://www.youtube.com/watch?v=tJOqGxN357cy-gems-apartment-act-as-tenant-and-98f123712315)

## [Apartment: (NOT compatible with Rails 6 onwards)](https://github.com/influitive/apartment)
**Apartment** provides a robust and flexible approach to multi-tenancy, offering features like schema isolation, tenant-specific migrations, and sharing data between tenants.

Offers both schema-based and database-based tenancy. 

May be a good fit for projects with distinct requirements. 

Hasn't received updates since 2019, which might cause compatibility issues with newer Rails applications. 

## [act_as_tenant: ](https://github.com/ErwinM/acts_as_tenant)
**act_as_tenant** is a lightweight and easy-to-use gem that offers basic tenanting functionality, including scoping methods and multi-tenant routing.

A simpler gem for straightforward projects. 
Focuses on scoping models to a tenant using a shared database strategy. 

Integrates well with Rails. 

## [ActiveRecord_multi_tenant](https://github.com/citusdata/activerecord-multi-tenant) 
**ActiveRecord_multi_tenant** leverages PostgreSQL’s schema features to isolate tenant data within a single database, providing a balance between flexibility and performance.

A more robust and potentially better suited for advanced projects than acts_as_tenant. 

Designed for complex multi-tenancy scenarios. 





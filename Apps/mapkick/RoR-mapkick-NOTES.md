

## [Mapkick Gem Spotlight | Ruby On Rails 7 Tutorial](https://www.youtube.com/watch?v=hck_SWp1cVA)
- ### [mapkick:Create beautiful JavaScript maps with one line of Ruby](https://chartkick.com/mapkick)
```

rails _7.2.1_ new mapkick-0 -d postgresql

rails g controller Home index show


#############
# CREDENTIALS
#

ACCESS:
mapbox PWD:
--
ciyJz.&_$3k!!ip
--
access-token-0
pk.eyJ1IjoiaGVpZGxlc3MiLCJhIjoiY20yZ2Zvc3hoMDByMzJqc2d1MWozbGJyaCJ9.dQLSI42M1Eo7wnXXzbjipA



EDITOR='subl --wait' ./bin/rails credentials:edit
--
mapkick: asdfasdfasdfaasdf

--

vi ./config/initializers/mapkick.rb
--
ENV["MAPBOX_ACCESS_TOKEN"] = Rails.application.credentials.dig(:mapkick)

--
mapkick: pk.eyJ1IjoiaGVpZGxlc3MiLCJhIjoiY20yZ2Zvc3hoMDByMzJqc2d1MWozbGJyaCJ9.dQLSI42M1Eo7wnXXzbjipA

```
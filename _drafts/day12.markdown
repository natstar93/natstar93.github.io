HTTP
Requests out, responses back

get: read data

post: change state of system


Can use global variable for $game this week but then never again


post '/shoot' do
  $game.p1.shoot params[:coordinate]
end

post '/place_ship' do
  coordinate = params[:coordinate]
  type = params[:ship_type]
end

POST localhost:4567/shoot

params => { coordinate: 'A1', ship_type: 'submarine' }




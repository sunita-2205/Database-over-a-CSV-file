# Database-over-a-CSV-file
impport requests
from bs4 import BeautifulSoup
import pandas
import argparse
import connect

parser=argparse.ArgumentParser()
parser.add_argument("--page_num_max", help="enter the number of pages to parse", type=int)
parser.add_argument("--dbname", help="enter the number of pages to parse", type=int)
args=parser.parse_args()

oyo_url="http://www.oyorooms.com/hotels-in-banagalore/?page="
page_num_MAX=args.page_num_max
scraped_info_list=[]
connect.connect(args.dbname)

for page_num in range(1, page_num_MAX):
    req=requests.get(oyo_url+str(page_num))
    content=req.content
    
    soup=BeautifulSoup(content, "html.parser")
    
    all_hotels=soup.find_all("div", {"class": "hotelCardListing"})
    
    for hotel in all_hotels:
        hotel_dict={}
        hotel_dict["name"]= hotel.find("h3", {"class": "ListingHotelDescription__hotelName"}).text
        hotel_dict["address"]=hotel.find("span",{"itemprop": "streetAddress"}).text
        hotel_dict["price"]=hotel.find("span",{"class":"ListingPrice__finalPrice"}).yext
        #try....except
        try:
            hotel_dict("rating")+hotel.find("span", {class"}: "hotelRating____ratingSummary"}).text
        except AttributeError:
            pass
        parent_amenities_element=hotel.find("div", {"class":"amenityWrapper"})
        
        amenities_list=[]
        for amenity in parent_amenities_element.find_all("div",{'class": "amenitiesWrapper__amenity"}):
            amenities_lsit.append(amenity.find("span", {"class": "d-bady-sm"}).text.strip())
            
        hotel_dict["amenities"]=',' .join(amenities_list[:-1])
        scraped_info_list.append(append(hotel_dict)
        
dataFrame=pandas.DataFrame(scraped_info_list0
dataFrame.to_csv("oyo.csv")
connect.get_hotel_info(args.dbname)

-- load the http module
local http = require("socket.http")
local ltn12 = require("ltn12")

-- Requests information about a document, without downloading it.
-- Useful, for example, if you want to display a download gauge and need
-- to know the size of the document in advance
--local res_body = "\'{\"ip\":\"115.159.156.234\"}\'"
local res_body = [[ {"ip":"1.1.1.1"} ]]
local response_body = { }

local res, code, response_headers = http.request {
    method = "POST",
    url="http://10.0.0.125:8889/ald_system/ips/get_ip_info",
    headers =  
      {  
          ["Content-Type"] = "application/json";  
          ["Content-Length"] = res_body:len();  
      },

      source = ltn12.source.string(res_body),  
      sink = ltn12.sink.table(response_body)
}  

if type(response_headers) == "table" then  
  for k, v in pairs(response_headers) do  
    print(k, v)  
  end  
end
print("Response body:")
if type(response_body) == "table" then
  print(table.concat(response_body))
else
  print("Not a table:", type(response_body))
end



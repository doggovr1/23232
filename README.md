handlers.GrantItemToPlayer = function GrantItemToPlayer(args)
{
    var GrantPlayerID = args.GrantPlayerID;
    var GrantItemID = args.GrantItemID;
    
    var GrantItemsToUserRequest = {
	    "PlayFabId" : GrantPlayerID,
	    "CatalogVersion": "Items",
	    "ItemIds": GrantItemID,
	    "Annotation": "purchase"
    };
    
    var GrantItemsToUserResult = server.GrantItemsToUserRequest(GrantItemsToUserRequest);
    
    
    return "Granted: " + GrantItemID + "To Playfab ID: " + GrantPlayerID;
}

handlers.BanPlayer = function BanPlayer(args)
{
    var PlayerID = args.PlayerID;
    var BanHours = args.BanHours;
    var BanReason = args.BanReason;

    server.BanUsers({Bans: [{DurationInHours: BanHours, PlayFabId: PlayerID, Reason: BanReason }] });
    return "Banned Player";
}


handlers.GivePlayerCurrency = function GivePlayerCurrency(args)
{
    var PlayerToGiveID = args.PlayerToGiveID;
    var AmountToGive = args.AmountToGive;
    //Make Sure You Set The VirtualCurrency Two Digit Code To Your Currency Code!!
    server.AddUserVirtualCurrency({PlayFabID: PlayerToGiveID, VirtualCurrency: "CN", Amount: AmountToGive});
    return "Gave Currency";
}

handlers.RemovePlayerCurrency = function RemovePlayerCurrency(args)
{
    var PlayerToRemoveID = args.PlayerToRemoveID;
    var AmountToRemove = args.AmountToRemove;
    //Make Sure You Set The VirtualCurrency Two Digit Code To Your Currency Code!!
    var SubtractUserVirtualCurrencyRequest = {
	    "PlayFabId" : PlayerToRemoveID,
	    "VirtualCurrency": "CN",
	    "Amount": AmountToRemove
    };

    var SubtractUserVirtualCurrencyResult = server.SubtractUserVirtualCurrency(SubtractUserVirtualCurrencyRequest);
    return "Removed Currency";
}

handlers.SendReport = function SendReport(args) 
{
    var ReporterID = args.ReporterID;
    var ReporterUsername = args.ReporterUsername;
    var ReportedID = args.ReportedID;
    var ReportedUsername = args.ReportedUsername;
    var ReportReason = args.ReportReason;
    
    var contentBody = {
          "content": null,
          "embeds": [
            {
              "color": 44543,
              "fields": [
                {
                  "name": "Reporter Username: " + ReporterUsername,
                  "value": "Profile Link: (**Sensitive Content**)" + "https://developer.playfab.com/en-us/" + "Your Title ID" + "/players/" + ReporterID + "/overview"
                },
                {
                  "name": "Reporter PlayFabID: " + ReporterID,
                  "value": "."
                },
                {
                  "name": "Reported Username: " + ReportedUsername,
                  "value": "Profile Link (**Sensitive Content**): " + "https://developer.playfab.com/en-us/" + "Your Title ID" + "/players/" + ReportedID + "/overview"
                },
                {
                  "name": "Reported PlayFabID: " + ReportedID,
                  "value": "."
                },
                {
                  "name": "Reason:",
                  "value": ReportReason
                }
              ],
              "author": {
              "name": "PlayFab Report",
              "icon_url": "https://images-ext-1.discordapp.net/external/4FPp03SVIn49x6Teu44L95kkP__Ui6fdKbna8g82_wM/%3FimageSize%3D512/https/www.nuget.org/profiles/PlayFab/avatar"
              },
              "footer": {
                "text": "Made By JokerJosh",
                "icon_url": "https://cdn.discordapp.com/avatars/791550177780563998/2ada0f85e2cc5f1fac3114dcae42a3bb.png?size=512"
              }
            }
          ],
          "attachments": []
        };
// your webhook URL
    var url = "https://discord.com/api/webhooks/1046316264613748777/5t6D1n4e01oTxJxHt-OhD5r5AhhyR3fnyjXEy3M1hBu7wXgqgeoiSlwxzHRNOzuWAEgk";
//no need to mess with anything here
    var method = "post";
    var contentType = "application/json";
    var headers = {};
    var responseString = http.request(url,method,JSON.stringify(contentBody),contentType,headers);
    
    return "SentMessage";
}

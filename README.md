# swift_record_tutorial

# Declaration

    var recordingSession: AVAudioSession!
    var audioRecorder: AVAudioRecorder!
    var audioPlayer: AVAudioPlayer!


# Get Path !Important

    func getDirectory() -> URL {
        let paths = FileManager.default.urls(for: .documentDirectory, in: .userDomainMask)
        let documentDirectory = paths[0]
        return documentDirectory
    }
    
# Record

    @IBAction func record(_ sender: UIButton) {
        //Check if we have an active recorder
        if audioRecorder == nil {
            //1. record를 새준다.
            numberOfRecords += 1
            //2. directory 주소 가져와서 audio 확장자 추가
            let filename = getDirectory().appendingPathComponent("\(numberOfRecords).m4a")
            
            //3. setting, p1 - format, p2 - rate, p3 - channel, p4 - quality
            let settings = [AVFormatIDKey: Int(kAudioFormatMPEG4AAC),
                            AVSampleRateKey: 12000,
                            AVNumberOfChannelsKey: 1,
                            AVEncoderAudioQualityKey: AVAudioQuality.high.rawValue]
            
            //4. record audio, Start audio recording
            do {
                audioRecorder = try AVAudioRecorder(url: filename, settings: settings)
                audioRecorder.delegate = self
                audioRecorder.record()
                
                buttonLabel.setTitle("Stop Recording", for: .normal)
            } catch {
                displayAlert(title: "Ups!", message: "Recording failed")
            }
            
        } else {
            //Stopping audio recording
            audioRecorder.stop()
            audioRecorder = nil
            
            UserDefaults.standard.set(numberOfRecords, forKey: "myNumber")
            tableView.reloadData()
            
            buttonLabel.setTitle("Record", for: .normal)
        }
        
    }
    
    
# Individual Play

    func tableView(_ tableView: UITableView, didSelectRowAt indexPath: IndexPath) {
        //중요
        let path = getDirectory().appendingPathComponent("\(indexPath.row + 1).m4a")
        do {
            audioPlayer = try AVAudioPlayer(contentsOf: path)
            audioPlayer.play()
        } catch {
            
        }
    }

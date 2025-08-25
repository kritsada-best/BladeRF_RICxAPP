## ====> srsRAN_Project <====

##

**config 1**

**path : ~/srsRAN_Project/lib/radio/uhd/radio_uhd_device.h**

**Original code**

    #if UHD_VERSION < 3140099
      return safe_execution([this, &sync_src, &clock_src]() {
        std::vector<std::string> time_sources = usrp->get_time_sources(0);
        if (std::find(time_sources.begin(), time_sources.end(), sync_src) == time_sources.end()) {
           on_error("Invalid time source {}. Supported: {}", sync_src, span<const std::string>(time_sources));
           return;
         }
        std::vector<std::string> clock_sources = usrp->get_clock_sources(0);
        if (std::find(clock_sources.begin(), clock_sources.end(), clock_src) == clock_sources.end()) {
          on_error("Invalid clock source {}. Supported: {}", clock_src, span<const std::string>(clock_sources));
          return;
        }
  
        usrp->set_time_source(sync_src);
        usrp->set_clock_source(clock_src);
      });
      

**code after configuration changes**

    #if UHD_VERSION < 3140099
      return safe_execution([this, &sync_src, &clock_src]() {
        // std::vector<std::string> time_sources = usrp->get_time_sources(0);
        // if (std::find(time_sources.begin(), time_sources.end(), sync_src) == time_sources.end()) {
          // on_error("Invalid time source {}. Supported: {}", sync_src, span<const std::string>(time_sources));
          // return;
        // }
        std::vector<std::string> clock_sources = usrp->get_clock_sources(0);
        if (std::find(clock_sources.begin(), clock_sources.end(), clock_src) == clock_sources.end()) {
          on_error("Invalid clock source {}. Supported: {}", clock_src, span<const std::string>(clock_sources));
          return;
        }
  
        // usrp->set_time_source(sync_src);
        usrp->set_clock_source(clock_src);
      });


## 
##

**config 2**

**path : ~/srsRAN_Project/lib/radio/uhd/radio_uhd_tx_stream.h**

**Original code**

    private:
      /// Receive asynchronous message timeout in seconds.
      static constexpr double RECV_ASYNC_MSG_TIMEOUT_S = 0.001;
      /// Transmit timeout in seconds.
      static constexpr double TRANSMIT_TIMEOUT_S = 0.001;


**code after configuration changes**

    private:
      /// Receive asynchronous message timeout in seconds.
      static constexpr double RECV_ASYNC_MSG_TIMEOUT_S = 0.001;
      /// Transmit timeout in seconds.
      static constexpr double TRANSMIT_TIMEOUT_S = 0;



##
##

**config 3**

**path : ~/srsRAN_Project/lib/radio/uhd/radio_uhd_rx_stream.h**

**Original code**

    private:
      /// Receive timeout in seconds.
      static constexpr double RECEIVE_TIMEOUT_S = 0.2f;
      /// Set to true for receiving data in a single packet.
      static constexpr bool ONE_PACKET = false;


**code after configuration changes**

    private:
      /// Receive timeout in seconds.
      static constexpr double RECEIVE_TIMEOUT_S = 0;
      /// Set to true for receiving data in a single packet.
      static constexpr bool ONE_PACKET = false;


##
##
**Build srsRAN_Project again**

**path: ~/srsRAN_Project**

    cd build
    cmake ../
    make -j $(nproc)
    sudo make install

##
##

## ====> SoapyBladeRF <====

**config 1**

**path : ~/SoapyBladeRF/bladeRF_Settings.cpp**

**Original code**

    bladeRF_SoapySDR::~bladeRF_SoapySDR(void)
    {
        SoapySDR::logf(SOAPY_SDR_INFO, "bladerf_close()");
        if (_dev != NULL) bladerf_close(_dev);
    }



**code after configuration changes**
  
    bladeRF_SoapySDR::~bladeRF_SoapySDR(void)
    {
        SoapySDR::logf(SOAPY_SDR_INFO, "bladerf_close()");
        if (_dev != NULL) bladerf_device_reset(_dev);
    }


## 






    

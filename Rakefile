require 'parallel'
require 'selenium/webdriver'

task :benchmark do
  browsers = [
    { platform: "OS X 10.11", browserName: "Chrome", version: "53" },
    { platform: "OS X 10.11", browserName: "Chrome", version: "52" },
    { platform: "OS X 10.11", browserName: "Firefox", version: "48" },
    { platform: "OS X 10.11", browserName: "Firefox", version: "47" },
  ]

  url = "http://#{ENV['SAUCE_USERNAME']}:#{ENV['SAUCE_ACCESS_KEY']}@ondemand.saucelabs.com:80/wd/hub"
  cname = File.open('CNAME').read

  benchmark = (ENV['BENCHMARK'] || '_benchmarks/*.md').split(',')
  Dir.glob(benchmark).each do |filename|
    puts "Benchmarking #{filename}"

    Parallel.each(browsers, in_threads: 5) do |caps|
      driver = Selenium::WebDriver.for(:remote, url: url, desired_capabilities: caps)

      driver.get('http://' + cname.strip + '/' + File.basename(filename, ".md"))
      wait = Selenium::WebDriver::Wait.new(:timeout => 60)

      wait.until do
        button = driver.find_element(:tag_name, 'button')
        button
      end

      button = driver.find_element(:tag_name, 'button')
      button.click

      wait = Selenium::WebDriver::Wait.new(:timeout => 600)
      wait.until do
        button = driver.find_element(:tag_name, 'button')
        button unless button.attribute('disabled')
      end

      driver.quit
    end
  end
end

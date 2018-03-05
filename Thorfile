require 'thor'
require 'activemerchant'
require 'logger'

ActiveMerchant::Billing::Base.mode = :test # or :production

ActiveMerchant::Billing::Gateway.logger = Logger.new(STDOUT)
ActiveMerchant::Billing::Gateway.wiredump_device = STDOUT

class Gateway < Thor
  desc 'search', 'Search for an ActiveMerchant Gateway by partial name match'
  def search(name)
    lower_name = name.downcase

    gateways = ::ActiveMerchant::Billing.constants.select do |gateway|
      gateway.downcase.match(lower_name)
    end

    gateways.each { |gateway| puts gateway }
  end

  desc 'purchase', 'purchase'
  def purchase(gateway, *options)
    parsed = parse_options(options)

    amount = parsed[:amount].to_i
    card = credit_card(parsed)

    instance = lookup_gateway(gateway, parsed)
    instance.purchase(amount, card, parsed)
  end

  desc 'authorize', 'authorize'
  def authorize(gateway, *options)
    parsed = parse_options(options)

    amount = parsed[:amount].to_i
    card = credit_card(parsed)

    instance = lookup_gateway(gateway, parsed)
    instance.authorize(amount, card, parsed)
  end

  desc 'capture', 'capture'
  def capture(gateway, *options)
    parsed = parse_options(options)

    authorization = parsed[:authorization]
    amount = parsed[:amount].to_i

    instance = lookup_gateway(gateway, parsed)
    instance.capture(amount, authorization, parsed)
  end

  desc 'void', 'void'
  def void(gateway, *options)
    parsed = parse_options(options)

    authorization = parsed[:authorization]

    instance = lookup_gateway(gateway, parsed)
    instance.void(authorization, parsed)
  end

  desc 'refund', 'refund'
  def refund(gateway, *options)
    parsed = parse_options(options)

    authorization = parsed[:authorization]
    amount = parsed[:amount].to_i

    instance = lookup_gateway(gateway, parsed)
    instance.refund(amount, authorization, parsed)
  end

  private

  def credit_card(options)
    options[:month] = options[:month].to_i
    options[:year] = options[:year].to_i
    options[:verification_value] = options[:verification_value].to_i

    ActiveMerchant::Billing::CreditCard.new(
      options.slice(
        :number,
        :month,
        :year,
        :first_name,
        :last_name,
        :verification_value,
        :brand
      )
    )
  end

  def parse_options(options)
    parsed = {}

    options.each do |option|
      key, value = option.split(':')
      parsed[key.to_sym] = value
    end

    parsed
  end

  def lookup_gateway(gateway, options)
    am_gateway = "::ActiveMerchant::Billing::#{gateway}".constantize
    am_gateway.new(options)
  end
end

